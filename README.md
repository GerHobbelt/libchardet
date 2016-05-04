libchardet - Mozilla's Universal Charset Detector C/C++ API
===

## License
Copyright (c) 2016 JoungKyun.Kim <http://oops.org> All rights reserved.

This program is under MPL 1.1 or LGPL 2.1

## Description
libchardet is based on Mozilla Universal Charset Detector library and, detects
the character set used to encode data.

[Original code](http://lxr.mozilla.org/seamonkey/source/extensions/universalchardet/) was writed by Netscape Communications Corporation, Techniques used by universalchardet are described at &lt;http://www-archive.mozilla.org/projects/intl/UniversalCharsetDetection.html&gt;.

libchardet see also John Gardiner Myers's [Encode-Detect-1.01](http://search.cpan.org/~jgmyers/Encode-Detect-1.01/)
perl module, and added C wrapping API, and library build environment with libtool.

From 1.0.5, libchardet was reflected single-byte charset detection confidence
algorithm of [uchardet](https://github.com/BYVoid/uchardet/) and new language models.
(Arabic, Danish, Esperanto, German, Spanish, Turkish, Vietnamese)


## Sample Codes

See also test directory of source code

```c
       #include <chardet.h>

       int main (void) {
            DetectObj *obj;

            if ( (obj = detect_obj_init ()) == NULL ) {
                 fprintf (stderr, "Memory Allocation failed\n");
                 return CHARDET_MEM_ALLOCATED_FAIL;
            }

            switch (detect ("안녕하세요", &obj)) {
                 case CHARDET_OUT_OF_MEMORY :
                      fprintf (stderr, "On handle processing, occured out of memory\n");
                      detect_obj_free (&obj);
                      return CHARDET_OUT_OF_MEMORY;
                 case CHARDET_NULL_OBJECT :
                      fprintf (stderr,
                                "2st argument of chardet() is must memory allocation "
                                "with detect_obj_init API\n");
                      return CHARDET_NULL_OBJECT;
            }

            printf ("encoding: %s, confidence: %f\n", obj->encoding, obj->confidence);
            detect_obj_free (&obj);

           return 0;
       }
```

or

```c
       #include <chardet.h>

       int main (void) {
            Detect    * d;
            DetectObj * obj;

            if ( (d = detect_init ()) == NULL ) {
                 fprintf (stderr, "chardet handle initialize failed\n");
                 return CHARDET_MEM_ALLOCATED_FAIL;
            }

            // for loop
            //detect_reset (&d);

            if ( (obj = detect_obj_init ()) == NULL ) {
                 fprintf (stderr, "Memory Allocation failed\n");
                 return CHARDET_MEM_ALLOCATED_FAIL;
            }

            switch (detect_handledata (&d, "안녕하세요", &obj)) {
                 case CHARDET_OUT_OF_MEMORY :
                      fprintf (stderr, "On handle processing, occured out of memory\n");
                      detect_obj_free (&obj);
                      return CHARDET_OUT_OF_MEMORY;
                 case CHARDET_NULL_OBJECT :
                      fprintf (stderr,
                                "2st argument of chardet() is must memory allocation "
                                "with detect_obj_init API\n");
                      return CHARDET_NULL_OBJECT;
            }

            printf ("encoding: %s, confidence: %f\n", obj->encoding, obj->confidence);
            detect_obj_free (&obj);
            detect_destroy (&d);

           return 0;
       }
```

## APIs
  * PHP Extension    - http://ftp.oops.org/pub/oops/php/extensions/mod_chardet/
  * PYTHON C Binding - http://ftp.oops.org/pub/oops/python/chardet/
  * PERL             - http://search.cpan.org/~jgmyers/Encode-Detect-1.01/Detect.pm
