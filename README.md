# bin
Helpful scripts to run on my home system.

## Make your own .ico file converter

There are many favicon.ico file converters out there, but anytime you download something from the
web you may be downloading malicious code. So, we want to have a favicon.ico generator to use in our
file system for any project we work on.

### Requirements

- [Python](https://www.python.org/downloads/)
- [Pillow](https://pillow.readthedocs.io/en/stable/)

### Strongly recommended

- [virtualenv](https://virtualenv.pypa.io/en/latest/installation/#)


### Get your feet wet using sys

Create a file with the following contents, ensuring that you add the interpreter so the
operating system knows what program to load and execute your script with. This is the first line
of code "#!/usr/bin/env python".  

`echo.py`
```python
#!/usr/bin/env python
import sys


# print the arguments, excluding the python file name
for arg in sys.argv[1:]:
    print(arg)
```

This will print out the variables you entered on the command line. You can execute this script from 
the terminal where the file is located with:

    $ python echo.py hey there
    hey
    there
    
Make the file executable:

    $ chmod +x echo.py
    
Then you can run the file as follows:

    $ ./echo.py hey there
    hey
    there
    
You now have your first executable file! This is a great starting point, but what if you want to
provide more information, such as variable names and types? This is where we will use `argparse`.


### Using argparse to create you .ico generator

The [argparse](https://docs.python.org/3/library/argparse.html) library is a great solution when you
want to have default values and add arguments with help text for the user should they need it. It also
allows for type checking, which is always good. The following is the file I generated to create
my executable scripts

I created a `bin` directory in my home directory, but you can place the code wherever you like, you
just have to map it properly in your bash file.
    
    
`createico`
```python
#!/usr/bin/env python
import argparse
import os

try:
    from PIL import Image
except ImportError:
    Image = None


def create_ico(input_f, output_f):
    if Image is None:
        print('\nMust install Pillow!\n\n')
        return

    img = Image.open(input_f)
    img.save(output_f)
    print('\nSaved {} file.\n\n'.format(output_f))


parser = argparse.ArgumentParser(description='Icon generator.')
parser.add_argument('--input', '-i', type=str, required=True,
                    help='Input file for conversion, required.')
parser.add_argument('--output', '-o', type=str, default='favicon.ico',
                    help='Output filename, default "favicon.ico".')

args = parser.parse_args()

file_exists = os.path.exists(args.input)
if file_exists:
    create_ico(args.input, args.output)
else:
    print('\nFile {} does not exist!\n\n'.format(args.input))

```

Make the file executable:

    $ chmod +x createico
    
Add the path of the file for your `bin` directory to your `.bashrc` or `.bash_profile`:

    # Setting PATH for user scripts
    PATH="${PATH}:${HOME}/bin"
    
    export PATH

Assuming you have Pillow installed, you should now be able to go to any directory that contains an
image file and run the following to generate a "favicon.ico" file:

    $ createicon -i favicon.png
    
Hope this helps and makes working with assets much easier!
