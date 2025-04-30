
# Born2beroot

![42](https://img.shields.io/badge/-42-black?style=for-the-badge&logo=42&logoColor=white)

<imagem_aqui>

![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black) ![Debian](https://img.shields.io/badge/Debian-D70A53?style=for-the-badge&logo=debian&logoColor=white) ![Shell Script](https://img.shields.io/badge/Shell_Script-121011?style=flat&logo=gnu-bash&logoColor=white) Git 	![Git](https://img.shields.io/badge/git-%23F05033.svg?style=for-the-badge&logo=git&logoColor=white) ![Markdown](https://img.shields.io/badge/markdown-%23000000.svg?style=for-the-badge&logo=markdown&logoColor=white)

**Born2beroot** is one of the Milestone 1 projects at [42 Porto](https://www.42porto.com). The main goal of this project is to build a Virtual Machine from scratch. We use `VirtualBox` as the virtualization software (more on that later), and `Debian` as the operating system.
Throughout the project, we learned key concepts such as `virtualization`, `Linux`, `UFW`, `SSH`, `Shell scripting`, `partitions`, and more. We also dealt with lower-level topics involving system administration'
Additionally, in the `bonus` section, we could build the foundations needed to have a `WordPress` website, defined `LVMGroup` prtitions, and install an additional protocol, in our case, `FTP`.

---

# Content

- [Summary](#Summary)
- [Functionalities](#Functionalities)
- [Usability](#Usability)
- [Structure](#Structure)
- [Installation](#Installation)
- [Skills](#Skills)
- [Feedback](#Feedback)
- [Contact](#Contact)

---

# Summary

**Libft** is a library in C that contains standard functions in C, like `strlen`, `atoi`, `strdup`, etc. Not only, it includes additional and bonuses functions that expands it's functionalities, like string manipulation, number convertion and linked lists manipulation.

This project was a great exercise to understand how the standard library in C programming language works, and a great opportunity to deepen the concepts we were exposed in the Piscine.

---

# Functionalities

## Libc functions:

- Characters manipulation: `ft_isalpha`, `ft_isdigit`, `ft_isalnum`, `ft_isascii`, `ft_isprint`, `ft_toupper`, `ft_tolower`.

- String manipulation: `ft_strlen`, `ft_strchr`, `ft_strrchr`, `ft_strncmp`, `ft_strnstr`, `ft_strlcpy`, `ft_strlcat`, `ft_strdup`.

- Memory manipulation: `ft_memset`, `ft_bzero`, `ft_memcpy`, `ft_memmove`, `ft_memchr`, `ft_memcmp`.

- Conversion and allocation: `ft_atoi`, `ft_calloc`.

## Additional functions:

- String manipulation: `ft_substr`, `ft_strjoin`, `ft_strtrim`, `ft_split`, `ft_itoa`, `ft_strmapi`, `ft_striteri`.

- File descriptors: `ft_putchar_fd`, `ft_putstr_fd`, `ft_putendl_fd`, `ft_putnbr_fd`.

## Bonuses:

- Linked lists manipulation: `ft_lstnew`, `ft_lstadd_front`, `ft_lstsize`, `ft_lstlast`, `ft_lstadd_back`, `ft_lstdelone`, `ft_lstclear`, `ft_lstiter`, `ft_lstmap`.

---

# Usability

## Compilation

To compile the library you can use the `make` command in the terminal:

```Bash
make
```

This will generate the `libft.a` file, which is the static library that has all the implemented functions.

## How to include in your project

To use the **Libft** in your project, include the header `libft.h` and link it during compilation:

```C
#include "libft.h"
```

And compile your project with:

```Bash
cc -Wall -Wextra -Werror -c your_program.c -o your_program
```

---

# Structure

libft/\
├── Makefile\
├── libft.h\
└── src/\
    ├── ft_atoi.c\
    ├── ft_bzero.c\
    ├── ft_calloc.c\
    ├── ft_isalnum.c\
    ├── ft_isalpha.c\
    ├── ft_isascii.c\
    ├── ft_isdigit.c\
    ├── ft_isprint.c\
    ├── ft_itoa.c\
    ├── ft_lstadd_back.c\
    ├── ft_lstadd_front.c\
    ├── ft_lstclear.c\
    ├── ft_lstdelone.c\
    ├── ft_lstiter.c\
    ├── ft_lstlast.c\
    ├── ft_lstmap.c\
    ├── ft_lstmap.c\
    ├── ft_lstnew.c\
    ├── ft_lstsize.c\
    ├── ft_memchr.c\
    ├── ft_memcmp.c\
    ├── ft_memcpy.c\
    ├── ft_memmove.c\
    ├── ft_memset.c\
    ├── ft_putchar_fd.c\
    ├── ft_putendl_fd.c\
    ├── ft_putnbr_fd.c\
    ├── ft_putstr_fd.c\
    ├── ft_split.c\
    ├── ft_strchr.c\
    ├── ft_strdup.c\
    ├── ft_striteri.c\
    ├── ft_strjoin.c\
    ├── ft_strlcat.c\
    ├── ft_strlcpy.c\
    ├── ft_strlen.c\
    ├── ft_strmapi.c\
    ├── ft_strncmp.c\
    ├── ft_strnstr.c\
    ├── ft_strrchr.c\
    ├── ft_strtrim.c\
    ├── ft_substr.c\
    ├── ft_tolower.c\
    └── ft_toupper.c\

---

# Installation

1. Clone the repository:
```Bash
git clone https://github.com/gabrielhdbarbosa/libft.git
```

2. Browse to the project dir:
```Bash
cd libft
```

3. Compile/Clean/Full clean/Remake/Bonus:
```Bash
make
```
```Bash
make clean
```
```Bash
make fclean
```
```Bash
make re
```
```Bash
make bonus
```

---

# Feedback
![Screenshot from 2025-04-14 14-47-37](https://github.com/user-attachments/assets/09ecb373-8fdd-478e-8552-0f3fbf13cf08)

---

# Contact

If you have any question or suggestion feel free to contact me!

- E-mail: ghrb2811@gmail.com

- LinkedIn: [gabrielhdb](https://www.linkedin.com/in/gabrielhdb/)

---

Made with 🫀 by me!

---
