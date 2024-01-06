# Convert PDF files to PNG files, one per page

I created this since this implementation with Rust is about 2x times faster than what I used previously: https://github.com/Dima-369/pdf2png-mac

On an M1 Macbook, this takes about 700 ms to convert a 5 MB PDF file with 12 pages - just a number, depends on PDF and hardware spec. Tested on macOS 14.0.

Download the `libpdfium.dylib` file from https://github.com/bblanchon/pdfium-binaries and add it next to the compiled `pdf2png` binary or specify `--library-directory`.

# Compile

```bash
cargo build --release
```

Then the executable will be under `target/release/pdf2png`.

# Help from `pdf2png -h`

Convert a PDF to image files, one image file per PDF page. It uses a default target width/height of 2000px per resulting image. This overrides existing image files in the output directory. Prints the PDF page count to stdout. If the PDF is password protected, exit with code 3

```
Usage: pdf2png [OPTIONS] <PDF_PATH>

Arguments:
  <PDF_PATH>  The PDF file to convert to images

Options:
  -f, --first-page-only
          Convert only first page without adding -0 suffix and do not print page count to stdout
  -p, --password <PASSWORD>
          The PDF password
      --prefix <PREFIX>
          The file prefix of the PNG files meaning the "foo" part for "foo-0.png" when converting "foo.pdf". The prefix can be changed here. If missing, the file name without the extension from the PDF file is taken
  -o, --output-directory <OUTPUT_DIRECTORY>
          The output directory where all the image files are saved to [default: .]
  -l, --library-directory <LIBRARY_DIRECTORY>
          The directory which contains the libpdfium.dylib file [default: .]
  -r, --resolution-pixels <RESOLUTION_PIXELS>
          The target width and maximum height in pixels. The width and height of the PNG files will not exceed this value [default: 2000]
  -h, --help
          Print help
```

# Notes

- Converting to PNGs is faster than JPEGs, so PNGs are used.
- If the PDF is password protected, pass a password via `-p` or `--password`. If no password is passed, it exits with error code 3 or if the password is incorrect.
