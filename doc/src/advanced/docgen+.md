# Notes on NVGT's documentation generator

## format of the src directory

* topics may be in .md or .nvgt format at this time. When NVGT becomes a c++ library as well, we may add ability to perform basic parsing of .h files.
* Directories are subsections and can be nested.
* Sections and subsections, starting at NVGTRepoRoot/doc/src, are scanned for topics with the following rules in mind:
    * If a .index.json file exists, this file is loaded and parsed to create the topic list. The json is a list containing either strings or 2-element lists. If a string, just indicates a topic filename. If a list, the first element is a topic filename string and the second is a topic display name string if the topic filename isn't adiquit.
    * If a .index.json file does not exist, topics/subsections are loaded in alphabetical order. A topic C will appear before a subsection F.
    * Unless a .index.json file specifies a topic display name, the topic filename minus the extension is used as the topic name. If the extension is .md, pythons .title() function is called on the topic basename to create the display name. Otherwise it is assumed that the topic basename.nvgt is an nvgt function name and the topic filename is used as the topic display name. Punctuation characters at the beginning of the display name are removed to provide for more flexible sorting. A subsection !C will show before a subsection F, but !C will display C thus allowing for multiple sorting levels without a .index.json file.
    * If there is a .MDRoot file present in a directory, the contents of that directory are output in new markdown/html documents instead of being appended to the main/parent documents, and a link to the new document is appended to the parent one.
    * If a topic filename contains an @ character right before the extension, that document is treated as a new markdown root document in the final output instead of being appended to the main/parent document, this is just a way to invoque the same as above but for a single file.
    * If a topic filename contains a + before the extension, the first line in the file minus markdown heading indicators will be used as the topic display name instead of a titlecased topic filename.
* .nvgt files contain embedded markdown within comments. Most markdown sintax is the same, accept that nvgt's docgen script replaces single line breaks with double lines so that each line is a markdown paragraph. Start a docgen comment with /**, and start a docgen comment with the above mentioned linefeed behavior disabled with /**\.
* Another embedded markdown comment within .nvgt files is the "// example:" comment, which will be replaced with "## example" in markdown.
* When parsing a .nvgt file, a "# topicname" markdown directive is added to the top of the output to avoid this redundant step in example functions.
* The docgen program creates a .chm file which requires parsing this markdown into html, and there may be reasons for removing embedded markdown indentation anyway. This resulted in an indentation rule where tabs are stripped from the document when passed to the python markdown package, spaces are not and can be used for things like nested lists.

## Installing the Microsoft HTML help compiler
Because of it's simple format and easy distrobution, we still prefer to generate the NVGT documentation as a .chm file (compressed HTML help). Unfortunately, the link to the html help workshop installer has been broken by Microsoft for a couple of years now. Fortunately, the installer for this program was archived from Microsoft's official website by the wayback machine. Until we get a better link, you should be able to [download it here](http://web.archive.org/web/20200312222543/http://download.microsoft.com/download/0/A/9/0A939EF6-E31C-430F-A3DF-DFAE7960D564/htmlhelp.exe), though it should be noted that only those wishing to rebuild nvgt's documentation from source will need this program.