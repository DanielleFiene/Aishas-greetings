Greeting Card Generator

## Project Overview

This Python project provides a way to generate both **generic** and **personalized greeting cards** using context managers. The project demonstrates the use of the `@contextmanager` decorator and the creation of custom context managers through classes.

The code enables you to create:
1. **Generic Greeting Cards** – These cards have a standard message read from a file (e.g., a pre-written thank you or birthday message) and include personalized names.
2. **Personalized Greeting Cards** – These allow the user to write custom messages inside the card, with a personalized greeting and signature.

## Features
- **Context Managers**: 
  - One context manager uses the `@contextmanager` decorator to manage resources when working with files.
  - Another context manager is implemented using a class with `__enter__` and `__exit__` methods.
- **File Handling**: The code reads from a file containing the generic card text and writes personalized cards to new text files.
- **Card Generation**: You can easily generate multiple cards with different messages for different recipients, ensuring the files are properly opened and closed without leaks.

---

## Code Overview

### 1. **Generic Card Context Manager** (`generic` function)
- **Purpose**: Reads a pre-existing message from a generic card template file, then writes the message with a personalized greeting and signature to a new file.
- **File Handling**: Opens the source card file in read mode (`r`) and the output file in write mode (`w`). After processing, it ensures the files are closed, even if an error occurs.

```python
@contextmanager
def generic(card_type, sender_name, recipient):
    open_generic_card = open(card_type, 'r')
    open_new_card = open(f'{sender_name}_generic.txt', 'w')
    try:
        open_new_card.write(f"Dear {recipient} \n")
        open_new_card.write(open_generic_card.read())
        open_new_card.write(f"\nSincerely,{sender_name}")
        yield open_new_card
    finally:
        open_generic_card.close()
        open_new_card.close()
```

### 2. **Personalized Card Context Manager** (`Personalized` class)
- **Purpose**: Allows for more personalized, hand-written messages by the sender. It starts with a custom greeting and ends with a signature.
- **File Handling**: Uses a custom context manager implemented with `__enter__` and `__exit__` methods to open, write to, and close the file.

```python
class Personalized:
    def __init__(self, sender, receiver, mode='w'):
        self.sender = sender
        self.receiver = receiver
        self.mode = mode
        self.file = open(f'{self.sender}_personalized.txt', self.mode)
    
    def __enter__(self):
        self.file.write(f'Dear {self.receiver},\n')
        return self.file
    
    def __exit__(self, *exc):
        self.file.write(f"\nSincerely, \n{self.sender}")
        self.file.close()
```

### 3. **Usage**
- The code allows you to create both **generic** and **personalized cards**. After creating the cards, it opens and displays the contents to confirm successful creation.

#### Example of usage:
```python
with generic('happy_bday.txt', 'Josiah', 'Remy') as order3:
    with Personalized('Josiah', 'Esther') as order4:
        order4.write("Happy Birthday!! I love you to the moon and back. Even though you’re a pain sometimes, you’re a pain I can't live without. I am incredibly proud of you and grateful to have you as a sister. Cheers to 25!! You’re getting old!")

with open('Josiah_generic.txt', 'r') as order3_readable:
    with open('Josiah_personalized.txt', 'r') as order4_readable:
        print(order3_readable.read())
        print('----------------------')
        print(order4_readable.read())
```

---

## What I Learned

### 1. **Context Managers**: 
   - Learned how to use context managers both via the `@contextmanager` decorator and through defining the `__enter__` and `__exit__` methods in a class.
   - Mastered the efficient handling of resources, ensuring files are always closed properly, even if errors occur.

### 2. **File Handling in Python**:
   - I practiced reading from and writing to text files, ensuring the correct file mode (`r` for read, `w` for write) was used.
   - Understood how to manage multiple file operations at the same time, such as opening different files within the same `with` block.

### 3. **Modularity and Reusability**:
   - By creating reusable context managers, I built code that can be easily modified for different types of cards and messages. This approach saves time and reduces redundancy.

---

## How to Run the Code

1. **Prerequisites**:
   - Python 3.x installed on your system.
   - A text file (e.g., `thankyou_card.txt`, `happy_bday.txt`) with pre-written generic card messages for the context manager to read from.

2. **Steps**:
   - Save the Python script in a file (e.g., `card_generator.py`).
   - Create the required text files for generic cards.
   - Run the script to generate personalized and generic cards.

---

## Conclusion

This project has helped me understand advanced Python concepts such as context managers and file handling, all while creating a practical application for generating personalized and generic greeting cards.
