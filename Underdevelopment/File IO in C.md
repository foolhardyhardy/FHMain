# File IO in C  
This script is written to explore how to achieve the common file IO task using only C and its standard library. And I written it by raise prototype problems and solve them.

## 1. Where is the file-related utility located in C? Where to find reference for it?
In C, file utility is provided in 
```
#include <stdio.h>
#include <wchar.h>
```
For reference about APIs provided by header files above, I personally prefer (cppreference.com)[cppreference.com].  

## 2. Where it means by "file" in C?
In the context of Operating System, "file" is an abstraction provided by the OS for user to save data to storage. And I think "file" in C is just like "file" in OS. This question is worthy of pointing out because in many other modern programming languages, "file" serve as more than just storage abstraction, a instance of "file" can be a physical file, a networking stream or in-memory buffer etc.. However, in C, a "file" instance is always bound to a physical file (with the exceptions of stdin, stdout, stderr). And you can only manipulate a "file" instance by a pointer to FILE. The FILE itself is inplementaion-dependent thus I think it's not fine to manipulate it directly.

## 3. Problem 1
Suppose there is a file "input" (we do know the content of this file), copy its content into another file named "output".
### Sample Solution:
```
#include <stdio.h>
#include <stdlib.h>

int main() {
    const char *input_file_name = "input";
    const char *output_file_name = "output";
    FILE *input_file = fopen(input_file_name, "r");
    FILE *output_file = fopen(output_file_name, "w");
    const int buffer_size = 512;
    unsigned char *buffer = (unsigned char*)malloc(buffer_size*sizeof(unsigned char));
    size_t n_read = 0;
    printf("Ready to read and write\n");
    do {
        n_read = fread((void*)buffer, sizeof(unsigned char), buffer_size, input_file);
        printf("Read: %ld\n", n_read);
        fwrite((void*)buffer, sizeof(unsigned char), n_read, output_file);
    } while(n_read == buffer_size);
    fclose(input_file);
    fclose(output_file);
    free(buffer);
    return 0;
}
```
