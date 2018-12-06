A sample for compilation by manual

```sh
export SRCFILE=function-instrumentation.c.orig
export DSTFILE=function-instrumentation
export CFLAGS=-g

cpp -DPOLICY_FILE $SRCFILE > $DSTFILE.yaml
cpp $CFLAGS $SRCFILE > $DSTFILE.c
clang $CFLAGS -S -emit-llvm $CFLAGS $DSTFILE.c -o $DSTFILE.ll
opt -load ../../Release/lib/LLVMLoom.so -loom -S $DSTFILE.ll -loom-file $DSTFILE.yaml -o $DSTFILE.instr.ll
FileCheck -input-file $DSTFILE.instr.ll $SRCFILE
llc -filetype=obj $DSTFILE.instr.ll -o $DSTFILE.instr.o
clang $DSTFILE.instr.o -o $DSTFILE.instr
./$DSTFILE.instr > $DSTFILE.output
FileCheck -input-file $DSTFILE.output $SRCFILE -check-prefix CHECK-OUTPUT
```
