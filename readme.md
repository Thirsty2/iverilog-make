To build iverilog-make with an entrypoint that runs make, 
run the build from the alpine312 subdirectory:

cd alpine312
docker build . -t iverilog-make

The above assumes Docker can find an alpine based image called iverilog.  

You can base the build on a particular image using the --build-arg IMAGE param:
docker build --build-arg IMAGE="docker.pkg.github.com/thirsty2/iverilog/iverilog:dockerfile-with-github-actions" . -t iverilog-make

When you run this container, it automatically invokes make in the current directory.

The -v (volume) command maps the current directory on your system to
the /home/ic directory inside the container.

Try this in the test directory:

~/iverilog-make/test$ ls
Makefile  hello.v  hello_tb.v  readme.txt
~/iverilog-make/test$ docker run -v $(pwd):/home/ic iverilog-make
iverilog -o hello_tb.vvp hello_tb.v
vvp hello_tb.vvp
VCD info: dumpfile hello_tb.vcd opened for output.
Hello World test complete
~/iverilog-make/test$ ls
Makefile  hello.v  hello_tb.v  hello_tb.vcd  hello_tb.vvp  readme.txt

If you have gtkwave installed, use it to view the .vcd file
~/iverilog-make/test$ gtkwave hello_tb.vcd

Run the container again, passing the clean command to the make entrypoint:

~/iverilog-make/test$ docker run -v $(pwd):/home/ic iverilog-make clean
rm hello_tb.vvp hello_tb.vcd
~/iverilog-make/test$ ls
Makefile  hello.v  hello_tb.v  readme.txt

