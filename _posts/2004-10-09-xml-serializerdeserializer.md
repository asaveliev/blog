---
title: Simple Serializer/Deserializer
---

```

public static string SerializeObject(object obj)
{
 MemoryStream temp = new MemoryStream();
 XmlSerializer serializer = new XmlSerializer(obj.GetType());
 serializer.Serialize(temp,obj);

 temp.Seek(0,SeekOrigin.Begin);
 byte[] byteArray = new byte[temp.Length];
 int count = 0;
 while (count < temp.Length)
 {
  byteArray[count++] = Convert.ToByte(temp.ReadByte());
 }
 ASCIIEncoding asciiEncoding = new ASCIIEncoding();
 char[] charArray = new char[asciiEncoding.GetCharCount(byteArray, 0, count)];
 asciiEncoding.GetDecoder().GetChars(byteArray, 0, count, charArray, 0);
 return new string(charArray);
}
public static object DeSerializeObject(string s,object obj)
{
 ASCIIEncoding asciiEncoding = new ASCIIEncoding();
 byte[] inp = asciiEncoding.GetBytes(s);
 MemoryStream temp = new MemoryStream();
 XmlSerializer serializer = new XmlSerializer(obj.GetType());

 temp.Write(inp,0, inp.Length);
 temp.Seek(0,SeekOrigin.Begin);
 return serializer.Deserialize(temp);
}
```

Update: apparently XmlSerializer is pretty limited in which types it can reflect, it's designed to fit current WDSL schema, which is limited itself. 
Well, not really, but in any case right now XmlSerializer can't serialize multidimentional arrays, for example. In the code above you can replace 
XmlSerializer with SoapFormatter, while keeping everything else exactly the same. That will generate different (SOAP) envelope and you will be able to serialize any type of data.