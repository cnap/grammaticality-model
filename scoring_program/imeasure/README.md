
# I-measure

This repository contains a Python implementation of the **I-measure**, a metric used for evaluating grammatical error correction systems. A full description of the method can be found in the following paper, which should be cited whenever you use the script in your work:

> Mariano Felice and Ted Briscoe. 2015. [**Towards a standard evaluation method for grammatical error detection and correction**](http://www.cl.cam.ac.uk/~mf501/pub/docs/2015-naacl.pdf). In Proceedings of the 2015 Conference of the North American Chapter of the Association for Computational Linguistics: Human Language Technologies (NAACL-HLT 2015), Denver, CO. Association for Computational Linguistics. (To appear)

# Requirements

These scripts were coded in Python 2.7 and require the `xml.etree.cElementTree` library (usually shipped with Python by default).

# Usage

## Simple usage

You can evaluate a system's output by issuing the following command:

`python ieval.py -hyp:system_output_file -ref:gold_standard_file`

where *system_output_file* is the plain-text tokenised output generated by an error correction system and *gold_standard_file* is an XML-formatted gold-standard file containing each source sentence with its errors and corrections. You can test the script using the example files provided:

`python ieval.py -hyp:example/sys.txt -ref:example/gold.xml`

This will print a table showing the results for the whole input corpus. Apart from the I-measure, the output will include complementary counts and measures that are useful for comparison and error analysis.

## Options

The full set of options accepted by the evaluation script is as follows:

`python ieval.py -ref:<file> -hyp:<file> [-nomix] [-max:<metric>] [-opt:sent|corpus] [-b:<n>] [-w:<n>] [-per-sent] [-v] [-vv]`

where:

Parameter | Description
----------|------------
-ref      | XML file containing gold standard annotations.
-hyp      | Plain text file containing sentence hypotheses (one per line).
-nomix    | Do not mix corrections from different annotators; match the best individual reference instead. By default, the scorer will mix such corrections in order to maximise matches. This option disables default behaviour.
-max      | Maximise scores for the specified metric: *dp*, *dr*, *df*, *dacc*, *dwacc*, *di*, *cp*, *cr*, *cf*, *cacc*, *cwacc* or *ci*. Preceding *d* is for detection, *c* for correction. Available metrics are: *tp* (true positives), *tn* (true negatives), *fp* (false positives), *fn* (false negatives), *p* (precision), *r* (recall), *f* (F measure), *acc* (accuracy), *wacc* (weighted accuracy), *i* (improvement on wacc). Default is **cwacc**.
-opt      | Optimise scores at the sentence or corpus level. Default is **sent**.
-b        | Specify beta for the F measure. Default is **1.0**.
-w        | Specify weight of true and false positives for weighted accuracy. Default is **2.0**.
-per-sent | Show individual results for each sentence.
-v        | Verbose output.
-vv       | Very verbose output.

These parameters can be specified in any order. Those within brackets are optional and will take their default values when omitted.

## Gold standard file format

Gold standard references should be provided in XML format according to the following DTD:

```
<!DOCTYPE GOLD-STANDARD [

<!ELEMENT scripts (script)>
<!ELEMENT script (sentence)>
<!ELEMENT sentence (text,error-list)>
<!ELEMENT text (#PCDATA)>
  <!-- Tokenised text of a sentence -->
<!ELEMENT error-list (error)+>
<!ELEMENT error (alt)+>
  <!-- Each of the errors in a sentence -->
<!ELEMENT alt (c)+>
  <!-- Each of alternatives for correcting an error -->
<!ELEMENT c (#PCDATA)>
  <!-- Each individual correction that makes up an alternative -->

<!ATTLIST script id CDATA #REQUIRED>
<!ATTLIST sentence id CDATA #REQUIRED>
<!ATTLIST sentence numann CDATA #REQUIRED>
  <!-- Total number of annotators for a sentence -->
<!ATTLIST error id CDATA #REQUIRED>
<!ATTLIST error req NMTOKEN #REQUIRED>
  <!-- Whether a correction is required for the error (yes/no)-->
<!ATTLIST error type NMTOKEN #REQUIRED>
  <!-- Error type string -->
<!ATTLIST alt ann CDATA #REQUIRED>
  <!-- Id of the annotator providing the alternative -->
<!ATTLIST c end CDATA #REQUIRED>
  <!-- Start position of a token requiring a correction in the original sentence  -->
<!ATTLIST c start CDATA #REQUIRED>
  <!-- End position of a token requiring a correction in the original sentence  -->
]>
```

Check out `example/gold.xml` for an example.

## Converting an .m2 file to XML annotation

You can use `m2_to_ixml.py` to convert an existing M² Scorer .m2 file to the XML format used by the I-measure evaluation script. Syntax is as follows:

`python m2_to_ixml.py -in:<m2-file> [-out:<xml-file>]`

This conversion script will do some basic automatic merging based on overlapping token positions. More sophisticated merging is required for better results.

# License

The MIT License (MIT)

Copyright (c) 2015 Mariano Felice

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

# Contact

If you have any questions, suggestions or bug reports, you can contact me at mariano dot felice at cl.cam.ac.uk.

