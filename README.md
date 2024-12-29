# SimpleLoader
An ELF Loader in C 

How to run :  Type command "make" in Linux shell to run the code

1. Global Variables:
   
    ● Elf32_Ehdr *ehdr : Pointer to the ELF header ( initialized to NULL ).
    ● Elf32_Phdr *phdr : Pointer to the program header ( initialized to NULL ).
    ● int fd: File descriptor for the ELF binary (initialized to -1 )..
    ● typedef int (*StartFuncType)(void): Function pointer to the entry point of the ELF
    binary
   
2. Cleanup:
   
    ● loader_cleanup(): This function is responsible for cleaning up resources:
    ○ This function frees the allocated heap memory for elf header and program header
    and initializes the reference variable to NULL.
    It also closes the file if it is open.
   
3. Main Loading and Execution Logic:
   
    ● **load_and_run_elf(char argv): This function does the loading and execution of the ELF
    binary. It performs the following steps:
    ○ It opens a read only file fd
    ○ Allocates memory for elf header and program header using malloc function .
    ○ It seeks the fd to the program header offset using lseek function
    ○ It iterates through every program header one by one and checks whether it is
    PT_LOAD . If it is then allocates virtual memory using mmap function ( we
    changed the NULL part to (void *)(uintptr_t)phdr[i].p_vaddr )
    ○ It then type casts the entry point address extracted from elf header to _start using
    (StartFuncType) defined globally
    ○ Result is printed
   
4. Main Function:
   
    ● **main(int argc, char argv): This function is the entry point of the program. It:
    1. Calls load_and_run_elf() to load and execute the ELF binary.
    2. Calls loader_cleanup() to release resources after execution.
       
5. System Calls:
   
    ● open(): Used to open the ELF file in read-only mode.
    ● read(): Used to read data from the file descriptor.
    ● lseek(): Used to change file offset.
    ● mmap(): Used to map the segments of the ELF file into memory.
    ● close(): Used to close the opened ELF file descriptor.
    ● malloc(): Used to allocate heap memory for the ELF header and program header.
    ● free(): Used to free the allocated heap memory.
    ● exit(): Used to terminate the program in case of errors.
   
6. Error Checks
   
    ● It provides error checks wherever necessary.
