---
title: "2018 layer7ctf WhatIsWrong"
date: 2019-05-23
categories: write-up
---

```csharp
using System;
using System.Text;
using System.Security.Cryptography;
using System.IO;
using System.Linq;

namespace WhatIsWrong
{
    public static class StringCipher
    {
        private const int Keysize = 256;

        private const int DerivationIterations = 1000;
        public static string Decrypt(string cipherText, string passPhrase)
        {
            var cipherTextBytesWithSaltAndIv = Convert.FromBase64String(cipherText);
            var saltStringBytes = cipherTextBytesWithSaltAndIv.Take(Keysize / 8).ToArray();
            var ivStringBytes = cipherTextBytesWithSaltAndIv.Skip(Keysize / 8).Take(Keysize / 8).ToArray();
            var cipherTextBytes = cipherTextBytesWithSaltAndIv.Skip((Keysize / 8) * 2).Take(cipherTextBytesWithSaltAndIv.Length - ((Keysize / 8) * 2)).ToArray();

            using (var password = new Rfc2898DeriveBytes(passPhrase, saltStringBytes, DerivationIterations))
            {
                var keyBytes = password.GetBytes(Keysize / 8);
                using (var symmetricKey = new RijndaelManaged())
                {
                    symmetricKey.BlockSize = 256;
                    symmetricKey.Mode = CipherMode.CBC;
                    symmetricKey.Padding = PaddingMode.PKCS7;
                    using (var decryptor = symmetricKey.CreateDecryptor(keyBytes, ivStringBytes))
                    {
                        using (var memoryStream = new MemoryStream(cipherTextBytes))
                        {
                            using (var cryptoStream = new CryptoStream(memoryStream, decryptor, CryptoStreamMode.Read))
                            {
                                var plainTextBytes = new byte[cipherTextBytes.Length];
                                var decryptedByteCount = cryptoStream.Read(plainTextBytes, 0, plainTextBytes.Length);
                                memoryStream.Close();
                                cryptoStream.Close();
                                return Encoding.UTF8.GetString(plainTextBytes, 0, decryptedByteCount);
                            }
                        }
                    }
                }
            }
        }

    private static byte[] Generate256BitsOfRandomEntropy()
    {
        var randomBytes = new byte[32]; // 32 Bytes will give us 256 bits.
        using (var rngCsp = new RNGCryptoServiceProvider())
        {
            rngCsp.GetBytes(randomBytes);
        }
        return randomBytes;
    }
}


    namespace EncryptStringSample
    {
        class Program
        {
            static void Main(string[] args)
            {
                string password = "TEFZRVI4e1RISVMgSVMgRkFLRSBGTEFHIEhBSEEhIX19ISFBSEFIIEdBTEYgRUtBRiBTSSBTSUhUezhSRVlBTCXNsoGTf94KxJs8YUlv3my6g+NubCug+Bimmvpij8GUawLsMZjNHrA0mAVkH5T3VHhxJ/lZqqcERcMQWo3wh3lIRlSiUxNDv5xKJD57f8ECw1YbIjLf+3Q2YC//rpOzEKvnKqP4ikk6vrQFVBMqmm6eD7nXq7oaoGrBSxJEz+9DIEzeP4z/CsRQm8d2dDPwZ5zM+JUdmgM54LNXlfyjwmnctqZYwOCGGt5VJJVj3K9nrPYmwFm1+RBXsd6zw/8TLlvnIl/sJ+ZgB4gShuNoOpJRDHHkQVbkZhFos43au1WuPgGwlrFdU8+gZDajS+cho1J0l1kObLrZ3htm5lJXl1od1hubv4+V/zk3pQQvG0U6kSCkWjM5shYZkhU6NZrddW/bJe5SGpoFW7jSxRkmDEFsz4Oz+BfMqPdMdgwEhTBE";
                string encryptedstring = "TEFZRVI4e1RISVMgSVMgRkFLRSBGTEFHIEhBSEEhIX19ISFBSEFIIEdBTEYgRUtBRiBTSSBTSUhUezhSRVlBTD5KFXSjwraTuJkmiJU8DB73nhajg2W0tWJ/VwLTH7I+";
                Console.WriteLine("Your decrypted string is:");
                string decryptedstring = StringCipher.Decrypt(encryptedstring, password);
                Console.WriteLine(decryptedstring);
                Console.WriteLine("Press any key to exit...");
            }
        }
    }
}
 ```
 layer7ctf 때 못풀었던 c#문제이다.그 당시에 write-up도 찾을 수 없었는데 지금 다시 분석해보니 aes암호화여서
 
 https://stackoverflow.com/questions/10168240/encrypting-decrypting-a-string-in-c-sharp여기서 복호화 코드를 찾아서 분석해서 얻은 키를 넣으니 
 슥-삭하고 플래그가 나왔다.
 
 
 플래그:LAYER7{MVVM Master}
