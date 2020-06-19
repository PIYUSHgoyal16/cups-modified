# CUPS Filters - 2020-06-11
__________________________________________________________

## INSTALLATION

**Prerequisites;** 
You'll need ANSI-compliant C and C++ compilers, plus a make program and POSIX-compliant shell (/bin/sh). For compiling and using this package CUPS, libqpdf (8.3.0 or newer), libjpeg, libpng, libtiff, freetype, fontconfig, liblcms(liblcms2 recommended), libavahi-common, libavahi-client, libdbus, and glib are needed. It is highly recommended, especially if non-PDF printers are used, to have at least one of Ghostscript, Poppler, or MuPDF.

Step 1: Clone the official cups repository.

    git clone https://github.com/OpenPrinting/cups-filters.git


Step 2: Move into the cups-filters directory and configure CUPS for your system through the usual "configure" script in the main CUPS Filters source directory, using the following command: 
    
    ./configure

Step 3: After configuration use the make command to build executables from the source code;

    make ENTER

Step 4: Once you have built the software you need to install it using the command;

    sudo make install

Read the file "INSTALL.txt" for detailed compile instructions.
___

## List of CUPS Filters

* bannertopdf
* commandtoescpx
* commandtopclx
* gstopdf
* gstopxl
* gstoraster
* **[imagetopdf](#imagetopdf)**
* imagetops
* imagetoraster
* mupdftoraster
* pclmtoraster
* **[pdftopdf](#pdftopdf)**
* [pdftops](#pdftops)
* [pdftoraster](#pdftoraster)
* rastertoescpx
* [rastertopclm](#rastertopclm)
* [rastertopclx](#rastertopclx)
* [rastertopdf](#rastertopdf)
* rastertops
* texttopdf
* texttops
* texttotext
* urftopdf

<!-- pdftoippprinter -->

___

<h2 id="imagetopdf"> IMAGETOPDF </h2>

1. ### INTRODUCTION

    This program is "imagetopdf". "imagetopdf" is a CUPS filter which reads
    a single image file, converts it into a PDF file and outputs it to stdout.

    This program accepts the following image file format;

    gif, png, jpeg, tiff, photocd, portable-anymap, portable-bitmap,
    portable-graymap, portable-pixmap, sgi-rgb, sun-raster, xbitmap,
    xpixmap, xwindowdump

    xbitmap, xpixmap and xwindowdump images are converted into png images by
    the "convert" command. Other kinds of image file format can be supported
    if the "convert" command support them.

    Output PDF file format conforms to PDF version 1.3 specification, and
    input image is converted and contained in the output PDF file as a binary format non-compression image.

    "imagetopdf" may outputs multiple pages if the input image exceeds page
    printable area.

    <br>

2. ### LICENSE

    "imagetopdf.c" is under the CUPS license. See the "COPYING" file.
    For other files, see the copyright notice and license of each file.

    <br>

3. ### COMMAND LINE
    "imagetopdf" is a CUPS filter, and the command line arguments, environment variables and configuration files are in accordance with the CUPS filter interface.

        imagetopdf <job> <user> <title> <num-copies> <options> [<filename>]

    "imagetopdf" simply ignores \<job> and \<user>. However, one should not miss out these arguments, as the command will result in error in case it is devoid of these arguments.

    **\<title>** is appended into the PDF dictionary as /Title.

    **\<num-copies>** specifies the number of document copies.

    **\<options>** is a CUPS option list. The detailed list of options along examples can be found [below](#imagetopdfoptions).

    **\<filename>** is an input image file name.

    When omit the **\<filename>**, "imagetopdf" reads an image file from **stdin**.

    <br>

4. ### ENVIRONMENT VARIABLES

    This program refers the following environment variable;

        PPD:  PPD file name of the printer.

    <br>

5. <h3 id="imagetopdfoptions"> COMMAND OPTIONS </h3>

    \<options> are delimited by space; boolean type CUPS options can be set by only adding the option key, other types are provided as pairs of key and value, \<key>=\<value>.

        ./imagetopdf job user title 1 '-o mirror -o Copies=10' flower.jpg >> output.pdf


    "imagetopdf" accepts the following CUPS standard options;

    <br>

    * #### fitplot
        Compared to the PostScript-based original CUPS filters there is a change of defaults: The imagetopdf and imagetoraster filters print in "scale-to-fit" mode (image is scaled to fill one page but nothing of the image being cut off) by default.

        This is done to support photo printing via AirPrint. The photo apps on Apple's iOS devices send print jobs as JPEG images and do not allow to set any options like "scaling" or the page size. With "scale-to-fit" mode set by default, the iOS photos come out on one page, as expected.
        
        To get back to the old behavior, supply one of the options "nofitplot" "filplot=Off", "nofit-to-page", or "fit-to-page=Off".

        | Default     | No Fitplot          |
        | -------- | -------------- |
        | ```./imagetopdf job user title 1 fitplot flower.jpg >> default.pdf ``` | ```./imagetopdf job user title 1 fitplot=Off flower.jpg >> fitplotOff.pdf``` |
        | ![default-1](https://user-images.githubusercontent.com/43112419/84997961-d49f1080-b16c-11ea-8ef5-c9ae91d0970f.jpg) | ![fitplotOff-1](https://user-images.githubusercontent.com/43112419/84997973-d9fc5b00-b16c-11ea-9e5f-b4460345159d.jpg) |

    <br>

    * #### mirror
        A mirror image is a reflected duplication of an image that appears almost identical, but is reversed in the direction perpendicular to the vertical axis. It is an image flipped with respect to vertical axis.     
        The mirror is a boolean option, which when set to True, produces a mirror image of the input image. Only specifying the mirror argument is enough as this is a boolean option and does not expect any analog value.  

        | Default     | Mirror          |
        | -------- | -------------- |
        | ```./imagetopdf job user title 1 fitplot flower.jpg >> default.pdf ``` | ```./imagetopdf job user title 1 mirror flower.jpg >> mirror.pdf``` |
        | ![default-1](https://user-images.githubusercontent.com/43112419/84997961-d49f1080-b16c-11ea-8ef5-c9ae91d0970f.jpg) | ![mirror-1](https://user-images.githubusercontent.com/43112419/84997988-de287880-b16c-11ea-91e9-f2d1d19378fe.jpg) |
        
    <br>

    * #### PageSize
        PageSize Option as the name suggests can be used to determine the PageSize over which the file is expected to be printed. The Default PageSize is "A4". However the other PageSize Options include;
        * A0
        * A1
        * A2
        * A3
        * A4
        * B
        * C
        * D
        * E
        * Letter
        * Legal
        * Tabloid

        <br>

        An Example to use the PageSize "Letter" is given below;

            ./imagetopdf job user title 1 PageSize=Letter flower.jpg >> output.pdf

        You can also provide a Custom Page Size, using the commands given in the table.

        | Default(A4)     | inch          | cm     | points (1/72 of an inch)          |
        | -------- | -------------- |-------- | ----------- |
        | ```./imagetopdf job user title 1 PageSize=A4 flower.jpg >> a4.pdf``` | ```./imagetopdf job user title 1 PageSize=Custom.4x6in flower.jpg >> inch.pdf``` |```./imagetopdf job user title 1 PageSize=Custom.4x8cm flower.jpg >> cm.pdf ``` | ```./imagetopdf job user title 1 PageSize=Custom.100x120 flower.jpg >> points.pdf``` |
        | ![a4-1](https://user-images.githubusercontent.com/43112419/85134840-bd3e5100-b25a-11ea-8df9-8837adfb6178.jpg) | ![inch-1](https://user-images.githubusercontent.com/43112419/85134812-b44d7f80-b25a-11ea-9e75-2308358cf681.jpg) | ![cm-1](https://user-images.githubusercontent.com/43112419/85134827-b9aaca00-b25a-11ea-859a-3c0b40ad7559.jpg) | ![points-1](https://user-images.githubusercontent.com/43112419/85134852-c16a6e80-b25a-11ea-9c31-e796f13a8167.jpg) |

        Note that the filter will scale the page image up or down to the size that makes you PDF fit onto the sheet.
    
    <br>

    * #### Copies
        This option can be used to produce multiple copies of the same image. It accepts a numerical value and creates that many number of copies of the inputed image. An Example to produce 10 copies of an image is given below;
         <br>

        ```./imagetopdf job user title 1 Copies=10 flower.jpg >> output.pdf```


    <br>
    
    * #### page-left, page-right, page-bottom, page-top
        I could not find these options in imagetopdf.c 

    <br>
    
    * #### OutputOrder
    
    <br>
    
    * #### Collate
    
    <br>
    
    * #### sides
    
    <br>
    
    * #### cupsEvenDuplex
    
    <br>
    
    * #### position
        This common line option decides the position of the image on the paper. The default position is **center**. Some of the positional arguments are given in the table, while others are listed below it. 
        | center | top     | bottom          | left     | right          |
        | --------| -------- | -------------- |-------- | ----------- |
        | ```./imagetopdf job user title 1 '-o fitplot=off position=center' flower.jpg >> center.pdf``` | ```./imagetopdf job user title 1 '-o fitplot=off position=top' flower.jpg >> top.pdf``` |```./imagetopdf job user title 1 '-o fitplot=off position=bottom' flower.jpg >> bottom.pdf ``` | ```./imagetopdf job user title 1 '-o fitplot=off position=left' flower.jpg >> left.pdf```|```./imagetopdf job user title 1 '-o fitplot=off position=right' flower.jpg >> right.pdf``` | 
        | ![center-1](https://user-images.githubusercontent.com/43112419/85134309-e9a59d80-b259-11ea-97da-dba839c71f6b.jpg)|![top-1](https://user-images.githubusercontent.com/43112419/85134362-faeeaa00-b259-11ea-9195-f5af2fbc8ffe.jpg) |![bottom-1](https://user-images.githubusercontent.com/43112419/85134315-ed392480-b259-11ea-8195-70e9236ce711.jpg) | ![left-1](https://user-images.githubusercontent.com/43112419/85134332-f1fdd880-b259-11ea-898c-b090c681415d.jpg)| ![right-1](https://user-images.githubusercontent.com/43112419/85134348-f6c28c80-b259-11ea-9bbd-caabc8805fa3.jpg) |

        
        The Other argument values we could pass include:
        * top-right
        * top-left
        * bottom-right
        * bottom-left
        <br> 

        Kindly Note that we have also used "fitplot=off" argument in order to demonstrate the position argument. Otherwise the image will fill the whole page and position wont make a difference. 

        <br>

    * #### ppi
       PPI, or pixels per inch, refers both to the fixed number of pixels that a screen can display and the density of pixels within a digital image. PPI is most useful in preparing files for printing. An image with a higher PPI tends to be higher quality because it has a greater pixel density. We can define ppi using "ppi" option.

            ./imagetopdf job user title 1 ppi=250 flower.jpg >> ppi1.pdf        
        <br>
        Both xppi and yppi can be set to different values using the following syntax; 
            
            ./imagetopdf job user title 1 ppi=250x500 flower.jpg >> ppi2.pdf
            
    <br>
    
    * #### scaling
    
    <br>

    
    * #### natural-scaling
        This boolean option when passed true, makes the zoom level zero. Hence it can be considered as a subset of "scaling" option where the passed numerical scaling factor is 0.

            ./imagetopdf job user title 1 natural-scaling flower.jpg >> naturalscaling.pdf
    
    <br>
    
    * #### orientation-requested
        Page orientation is the way in which a rectangular page is oriented for normal viewing. The two most common orientations are "Portrait" and "Landscape". The other two options provided by "imagetopdf" filter are "reverse-landscape" and "reverse-portrait".

        | Portrait     | Landscape          | reverse-portrait     | reverse-landscape          |
        | -------- | -------------- |-------- | ----------- |
        | ```./imagetopdf job user title 1 orientation-requested=Portrait flower.jpg >> portrait.pdf``` | ```./imagetopdf job user title 1 orientation-requested=Landscape flower.jpg >> landscape.pdf``` |```./imagetopdf job user title 1 orientation-requested=reverse-portrait flower.jpg >> reversePortrait.pdf ``` | ```./imagetopdf job user title 1 orientation-requested=reverse-landscape flower.jpg >> reverseLandscape.pdf``` |
        | ![portrait-1](https://user-images.githubusercontent.com/43112419/85009785-a1fd1400-b17c-11ea-9ea5-672878fa173f.jpg) | ![landscape-1](https://user-images.githubusercontent.com/43112419/85009751-96115200-b17c-11ea-977b-d6064d1d431c.jpg) | ![reversePortrait-1](https://user-images.githubusercontent.com/43112419/85009803-ab867c00-b17c-11ea-8e4f-ee8023025a17.jpg) | ![reverseLandscape-1](https://user-images.githubusercontent.com/43112419/85009796-a75a5e80-b17c-11ea-84fd-15c8e2101a85.jpg) |

    <br>

    * #### landscape
        The default orientation is auto-detected by imagetopdf filter according to the length and width of the input image, resulting in the best possible way to fill one page but nothing of the image being cut off. However,To set default Landscape Orientation, supply one of the options "landscape=yes", or "landscape=true".
        
        Note that the option "orientation-requested" is given priority over "landscape" option, i.e. if we provide both options, "orientation-requested" will be used to determine the final orientation.
        <br>

            ./imagetopdf job user title 1 landscape=yes flower.jpg >> output.pdf


    <br>

6. ### KNOWN PROBLEMS

    Problem:\
        PBM and SUN raster images can not be printed.\
    Solution:\
        Due to the CUPS libcupsimage library's bug. Update the CUPS on your system.

___

<h2 id="pdftopdf"> PDFTOPDF </h2>

1. ### INTRODUCTION

    The pdftopdf filter depends on libqpdf to read and write PDF files.

    It replaces and imitates the pstops filter in the PDF-based workflow.
    A similar filter (which can serve as behavior reference)
    is called "cgpdftopdf" in OS X (not open source).

3. ### License

    pdftopdf is released under the MIT license.

    The required libqpdf is available under version 2.0 of the Apache License,
    e.g. here: https://github.com/qpdf/qpdf

3. ### COMMAND LINE

    pdftopdf follows the usual CUPS filter calling conventions, i.e.

        pdftopdf <job> <user> <title> <num-copies> <options> [<filename>]

    together with the environment variables "PPD" and "CLASSIFICATION".
    
    **\<title>** is appended into the PDF dictionary as /Title.

    **\<num-copies>** specifies the number of document copies.

    **\<options>** is a CUPS option list. The detailed list of options along examples can be found [below](#pdftopdfoptions).

    **\<filename>** is an input image file name.

    When omit the **\<filename>**, "imagetopdf" reads an image file from **stdin**.
    Internally this will write the data to a temporary file, because
    the PDF format cannot be processed in a streaming fashion.
4. <h3 id="pdftopdfoptions"> COMMAND OPTIONS </h3>

    "pdftopdf" processes the following standard command-line and/or PPD options:

    * #### Copies
        This option can be used to produce multiple copies of all the pages of a pdf file. It accepts a numerical value and creates that many number of copies of the inputed file. 

        | Default  | Multiple Copies|
        | -------- | -------------- |
        | ```./pdftopdf job user title 1 fitplot=false openprinting.pdf >> copies.pdf``` | ```./pdftopdf job user title 1 Copies=3 openprinting.pdf >> copies.pdf``` |
        | [default.pdf](https://github.com/PIYUSHgoyal16/test/files/4804357/output.pdf) | [copies.pdf](https://github.com/PIYUSHgoyal16/test/files/4804359/copies.pdf) |

        Note that if we provide **Copies=0**, then one copy will be outputed rather than a blank pdf file.

        ppd will only override, when commandline parameter was 1.
        <br>

    * #### fitplot / fit-to-page / ipp-attribute-fidelity

        | Default  | Fit-to-page    |
        | -------- | -------------- |
        | ```./pdftopdf job user title 1 fitplot=false openprinting.pdf >> copies.pdf``` | ```./pdftopdf job user title 1 fitplot=true openprinting.pdf >> copies.pdf``` |
        | [default.pdf](https://github.com/PIYUSHgoyal16/test/files/4804357/output.pdf) | [fitplot.pdf](https://github.com/PIYUSHgoyal16/test/files/4804362/fitplot.pdf) |

        <br>
    
    * #### landscape

        | Landscape=Off  | Landscape    |
        | -------- | -------------- |
        | ```./pdftopdf job user title 1 landscape=off openprinting.pdf >> landscape.pdf``` | ```./pdftopdf job user title 1 landscape openprinting.pdf >> landscape.pdf``` |
        | [landscapeoff.pdf](https://github.com/PIYUSHgoyal16/test/files/4804357/output.pdf) | [landscape.pdf](https://github.com/PIYUSHgoyal16/test/files/4804360/landscape.pdf) |

        <br>

    * #### orientation-requested

        | orientation-requested=3     | orientation-requested=4          | orientation-requested=5     | orientation-requested=6          |
        | -------- | -------------- |-------- | ----------- |
        | ```./imagetopdf job user title 1 orientation-requested=Portrait flower.jpg >> portrait.pdf``` | ```./imagetopdf job user title 1 orientation-requested=Landscape flower.jpg >> landscape.pdf``` |```./imagetopdf job user title 1 orientation-requested=reverse-portrait flower.jpg >> reversePortrait.pdf ``` | ```./imagetopdf job user title 1 orientation-requested=reverse-landscape flower.jpg >> reverseLandscape.pdf``` |
        | [orientationRequested3.pdf](https://github.com/PIYUSHgoyal16/test/files/4804357/output.pdf) | [orientationRequested4.pdf](https://github.com/PIYUSHgoyal16/test/files/4804360/landscape.pdf) | [orientationRequested5.pdf](https://github.com/PIYUSHgoyal16/test/files/4804357/output.pdf) | [orientationRequested6.pdf](https://github.com/PIYUSHgoyal16/test/files/4804360/landscape.pdf) |

    <br>

    * #### PageSize / page-size / MediaSize / media-size
    * #### page-left / page-right / page-bottom / page-top
    * #### media-top-margin / media-left-margin / media-right-margin / media-bottom-margin
    * #### Duplex / JCLDuplex / EFDuplex / JD03Duplex / sides
    * #### number-up / number-up-layout
    * #### page-border
    * #### OutputOrder / OutputBin / DefaultOutputOrder / page-delivery
    * #### page-label
    * #### page-set
    * #### page-ranges
    * #### MirrorPrint / mirror
    * #### emit-jcl
    * #### position
    * #### Collate / multiple-document-handling / sheet-collate
    * #### cupsEvenDuplex
    * #### cupsManualCopies  # via ppd

5. ### Additional (non-standard) options

    1) Booklet printing

        booklet=Off/On/Shuffle-Only

        "On" also tries to set DuplexTumble (two-sided-short-edge) and forces number-up=2

        booklet-signature=(multiple of 4, or default: -1 to use "all pages")

    2) Page autorotate

        pdftopdf automatically rotates pages to the same orientation,
        instead of (e.g. fitplot) scaling them down unrotated.
        This behavier can be controlled by

        pdfAutorotate / nopdfAutorotate

        Specifically, if a PDF file contains pages with page width greater than
        page height (a landscape page), such pages are automatically rotated
        anticlockwise by 90 degrees, unless the PPD file specifies
        "*LandscapeOrientation: Minus90". In this case, clockwise rotation is used.
        To turn off the feature on a job-by-job basis use

        lp -d <print_queue_name> -o nopdfAutorotate <document>

        On a per-queue basis use

        -o nopdfAutorotate-default

        as an option to lpadmin.

        When the 'landscape' or 'orientation-requested=4' (or =5) option of CUPS is
        given, the pdfAutorotate processing will adjust and accordingly rotate the
        non-landscape pages are rotated instead.

        Note: Some pages might end up 180 degree rotated (instead of 0 degree).
        Those should probably be rotated manually before binding the pages together.

    3) Method of flattening interactive PDF forms and annotations.

    Some PDF files (like application forms) contain interactive forms
    which the user can fill in inside a PDF viewer like evince. The filled
    in data is not integrated in each page of the PDF file but stored in
    an extra layer. Due to this the data gets lost when applying
    manipulations like scaling or N-up to the pages. To prevent the loss
    of the data pdftopdf flattens the form before doing the
    manipulations. This means the PDF will be converted into a static PDF
    file with the data being integral part of the pages.

    The same flattening is needed for annotations in PDF files.

    By default the actual flattening work is done by QPDF, as QPDF is also
    doing everything else in pdftopdf. This way no external utilities need
    to be called and so extra piping between processes and extra PDF
    interpreter runs are avoided which makes the filtering process faster.

    As we did not test the new QPDF-based form-flattening with thousands
    of PDF files yet and it has not been available to actual users yet it
    is possible that there are still some bugs. To give users a
    possibility to work around possible bugs in QPDF's form flattening, we
    have introduced an option to get back to the old flattening by the
    external tools pdftocairo or Ghostscript.

    The selection of the method is done by the "pdftopdf-form-flattening"
    option, setting it to "auto", "qpdf", "pdftocairo", "ghostscript",
    "gs", "internal" or "external":

    Per-job:           lpr -o pdftopdf-form-flattening=pdftocairo ...
    Per-queue default: lpadmin -p printer -o pdftopdf-form-flattening-default=gs
    Remove default:    lpadmin -p printer -R pdftopdf-form-flattening-default

    By default, pdftopdf uses QPDF if the option is not supplied, also the
    settings "auto" and "internal" make QPDF being used. "external"
    auto-selects from the two external utilities, trying pdftocairo at
    first and on failure Ghostscript. If the selected utility fails, the
    form stays unflattened and so the filled in data will possibly not get
    printed.

6. ### Native PDF Printer / JCL Support

Note that for most modern native PDF printers JCL is not needed any
more as they are controlled via IPP. For these the PPD files get
auto-generated by the support of CUPS and cups-filters for driverless
IPP printing.

pdftopdf will emit JCL when provided with a PPD file that includes the
"*JCLToPDFInterpreter:" keyword.

This enables for hardware copy generation and device collate; e.g. with PJL:

*JCLBegin:           "<1B>%-12345X@PJL JOB<0A>"
*JCLToPDFInterpreter: "@PJL ENTER LANGUAGE = PDF <0A>"
*JCLEnd:             "<1B>%-12345X@PJL EOJ <0A><1B>%-12345X"

For each marked option, the prefixed "pdftopdfJCL<option name>" keywords
can also be used to send raw JCL strings for that option.
These keywords also include *pdftopdfJCLBegin and *pdftopdfJCLCopies,
This allows the use of the same PPD for PDF- and PS-based workflows,
as pdftopdfJCL... will not be read in the PS case.

When the PPD contains the "Copies" keyword, pdftopdf will detect the use
of PJL and has special code which adds "@PJL SET COPIES=...",
or "@PJL SET QTY=...", respectively.

Other JCL code can be injected via "*JCLOpenUI: ..." ... "*JCLCloseUI: ...".

7. ### Special PDF comments

pdftopdf adds comments to the pdf preamble that might esp. be of use
to subsequent filters, e.g.

  % This file was generated by pdftopdf
  %%PDFTOPDFNumCopies : 1
  %%PDFTOPDFCollate : false

The "NumCopies" and "Collate" values refer to the expected device/hardware
copies, i.e. when pdftopdf's soft-copy generation did not handle this options.

8. ### Limitations

pdftopdf does not support functions that are not related to printing
features, including interactive features and document interchange features.
Many of these operators and sections are just ignored.
Some of these may be output, but those functions are not assured.

Most notable is the use of AcroForms; their content will not be printed
if any non-trivial processing by pdftopdf is involved (e.g. "fitplot").
This only occurs when a file is printed directly, e.g. by "lpr".

Usual PDF viewer applications (xpdf, evince, acroread, ghostscript, ...)
will hardcopy the form content into printable pdf operations,
when choosing to print such a document.

9. ### Known issues

- Borders, esp. in the "number-up=1 fitplot=false"-case might be drawn
  at incorrect locations.

- JCL documentation is sparse.
  The imagetopdf or old pdftopdf documentation contains a tad more information.

- Missing AcroForm-content might surprise users printing PDF files directly /
  from the command-line (see the Limitations section, above).




___

<h2 id="pdftops"> PDFTOPS </h2>

___

<h2 id="pdftoraster"> PDFTORASTER </h2>

___

<h2 id="rastertopclm"> RASTERTOPCLM </h2>

___

<h2 id="rastertopclx"> RASTERTOPCLX </h2>

___

<h2 id="rastertopdf"> RASTERTOPDF </h2>

