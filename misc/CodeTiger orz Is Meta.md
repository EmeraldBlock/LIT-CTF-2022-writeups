# misc/CodeTiger orz Is Meta

## Challenge

can you find the flag from this amazing codetiger fan picture!!

[codetigerfanpic.png](https://drive.google.com/uc?export=download&id=16deZrP2tRCF0HxevYHoGXsEXr_xAki3v)

## Solution

This seems to be a simple steganography challenge, so we just open the file in a text editor. Immediately, we see a block of XML:
```xml
<x:xmpmeta xmlns:x='adobe:ns:meta/' x:xmptk='Image::ExifTool 12.36'>
<rdf:RDF xmlns:rdf='http://www.w3.org/1999/02/22-rdf-syntax-ns#'>

 <rdf:Description rdf:about=''
  xmlns:dc='http://purl.org/dc/elements/1.1/'>
  <dc:description>
   <rdf:Alt>
    <rdf:li xml:lang='x-default'>t1g2r_</rdf:li>
   </rdf:Alt>
  </dc:description>
  <dc:rights>
   <rdf:Alt>
    <rdf:li xml:lang='x-default'>orz}</rdf:li>
   </rdf:Alt>
  </dc:rights>
  <dc:title>
   <rdf:Alt>
    <rdf:li xml:lang='x-default'>LITCTF{c0de_</rdf:li>
   </rdf:Alt>
  </dc:title>
 </rdf:Description>
</rdf:RDF>
</x:xmpmeta>
```
Just string the text together to get...

## Flag

`LITCTF{c0de_t1g2r_orz}`
