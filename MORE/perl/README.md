# Perl

## Venerable Variables:
- [passVariables](passvariable.md): take an variable, and put it's reference into another variable, and then print out the value.

## Messing with Arrays:
- [passArray](passarray.md): Make an array in one subroutine, and pass it to another subroutine. Then separate out the variables in the array with commas and print.
- [passArrayOfArray](passarrayofarray.md): make a 2d array, pass it to another subroutine, and separate the variables of each array with colons and print
- [copyFileToAofA](copyfiletoaofa.md): enter in a file that is tab delimitated, this script will break apart the tab delimitated cells, and convert each cell to variables. From all that it will create an array of arrays (2D array) and pass it to another subroutine that will print out all the variables with colon's in between.
- [RefOfArrays](refofarrays.md): ways you can use references of arrays.
- [Arrays to Lists](arrays-to-lists.md): what happens when you try and convert an array to a list.
- [ref/unref-ing Arrays](refunref-ing-arrays.md): how to reference arrays and get them back
- [transposeArray](transposearray.md): How to rotate a matrix
- [transposeArray2](transposearray2.md): Use the transpose method to convert columns in a spreadsheet into separate sheets
- [grabnPrint](grabnprint.md): In one sub open a file and put into an array, pass the reference of the array to another sub and do stuff

## Flailing around with Hashes:
- [matchHashes-a+b](matchhashes-ab.md): take two hash tables; a and b. make a new hash table that is the combined value of the two. If a and b have the same variables, use the value of the variable in the b hash as the value for the new hash table.
- [matchHashes-a_with_b_values](matchhashes-a_with_b_values.md): here we take the two hashes a and b, and add values from b to a IF the variable exists in a. Thus the output has only the items from a, but if b had the same item, then we get b's value for that idem overwritten over a.
- [StringHashes](string-hashes.md): with two hash's, one is the key for the other, and string them together
- [passHash](passhash.md): make two different hashes, in one make a normal hash and pass it out of a sub, in the other make an anonymous hash and pass it out of the subroutine. (they both end up being anonymous after they are passed out of the sub. Then accept it into another sub and print two ways; one in an OO way, and the other has a more normal looking code way of doing it.
- [passHash2](passhash2.md): Create a hash, and run two different subroutines that add to the hash. Do this by passing references to a hash around.
- [copyFileToHash](copyfiletohash.md): Take a tab delimitated text file, and convert it into a hash table. Then pass the reference for that hash table to another subroutine and print out the hash table.
- [passArraytoHash](passarraytohash.md): take an array, stick it into a hash, then erase the contents of the array and refill it up with something different, and then stick the new value into the array. (thus making a long list of arrays). Then print everything out.

## Ogling over OO:
- [Making and Referencing Modules](making-and-referencing-modules.md): Discusses how to create a module and how to link to it.  (less of a code example, and more of a how-to)
- [Extending the library path](http://www.perlhowto.com/extending_the_library_path): Perl Modules normally have to go in very specific places.  This tells you want to do if you want to store them someplace else. 

## Opening/Printing Stuff
- [doOrDie](doordie.md): Open a file and properly exit if the file is not there.
- [listDir](listdir.md): Look at a directory via perl, and print it out.
- [listDirViaOpen](listdirviaopen.md): Look at a directory by running a shell command within perl, and print results.
- [readInFile](readinfile.md): Open a file in one subroutine, then print it out within another.
- [usage](usage.md): All scripts should have usage statements, here's an example for how to do this.

## Mod_perl:
- [readGET](readget-1.md): grab a variable in the uri, and present it on the page
- [uriVars](urivars.md): how to feed in multiple variables in a uri
- [listDir2](listdir2.md): Look at a directory and print it out. 
- [AutoHTML](autohtml.md): using CGI:: to automatically enter in your html headers
- [FormSubmit](formsubmit.md): provide a form, submit it and do action against it. 

## Random tools and functions
- [Time](timepl.md): uses the localtime function to come up with date, time, second information.
- [Code2HTML](https://www.palfrader.org/code/code2html/): tool that converts perl script to color coded html.  Used in these pages to document code.  There is also tohtml
- [ping_sweeper](ping_sweeper.md): scans a list of sites from a file.
- [cisco_config_grabber](cisco_config_grabber.md): grabs configs from a cisco asa firewall.
- [getipaddress.pl](getipaddress.md): grabs your local IP, and submits it to eDNS to update your local DNS name.

## Perl Notes:
- [Installing CPAN](installing-cpan.md): Yea, you can install each perl module one by one, and remember when to upgrade them, or you could let this fancy app do if for you.
- [Perl Functions](https://perldoc.perl.org/functions#Alphabetical-Listing-of-Perl-Functions): a listing of the perl functions including `shift`, `push`, `print`, `exit`, `chomp`, and many more....
- [PerlMeme.org](http://www.perlmeme.org/): "is a collection of Frequently Asked Questions, "How To" documents, and tutorials about the very cool Perl programming language."

