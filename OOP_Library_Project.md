# 🎯 OOP Master Project: Library Management System

**Objective:** Build a complete library system using ALL Python OOP concepts

**Duration:** 3-5 hours  
**Difficulty:** Intermediate to Advanced

---

## 📚 Project Overview

Build a library system that manages:
- Different types of items (Books, Magazines, DVDs)
- Different types of users (Student, Teacher, Guest)
- Borrowing rules with different periods
- Late fee calculations
- Search functionality

---

## 🎓 Concepts one will Master

| Concept | Where You'll Use It |
|---------|---------------------|
| Classes & Objects | Everything! |
| Encapsulation | `_title`, `__item_id`, `_borrowed_items` |
| Inheritance | Book/Magazine/DVD from LibraryItem |
| Polymorphism | `get_borrow_period()`, `get_fine_rate()` |
| Abstract Classes | `User` base class |
| @property | Getters for title, item_id |
| @classmethod | `get_total_items()` |
| @staticmethod | `generate_item_id()` |
| Special Methods | `__str__`, `__eq__`, `__lt__` |
| super() | In child class `__init__` |
| Multiple Inheritance | Optional advanced feature |

---

## 📁 Project Structure
```
library_system/
│
├── library_item.py # Base LibraryItem class
├── items.py # Book, Magazine, DVD classes
├── user.py # User abstract class and subclasses
├── borrow_record.py # BorrowRecord class
├── library.py # Library management class
└── main.py # Test your system
```
---

---

# Level 1: Foundation (Encapsulation + Basic Classes)

## Task 1.1: Create LibraryItem Base Class

**File:** `library_item.py`

```python
# Your task: Fill in the blanks

class LibraryItem:
    """Base class for all library items"""
    
    total_items = 0  # Class variable to track all items
    
    def __init__(self, title, item_id):
        """
        Initialize a library item
        
        Args:
            title (str): Title of the item
            item_id (str): Unique identifier
        """
        self._title = title  # Protected attribute
        self.__item_id = item_id  # Private attribute (name mangled)
        self._is_borrowed = False
        LibraryItem.total_items += 1
    
    # TODO: Create @property for title (getter only, no setter)
    # Hint:
    # @property
    # def title(self):
    #     return self._title
    
    # TODO: Create @property for item_id (getter only)
    
    # TODO: Create @property for is_borrowed (getter only)
    
    # TODO: Create method borrow()
    # Should:
    # - Check if already borrowed
    # - Set _is_borrowed to True if not borrowed
    # - Return True if successful, False otherwise
    
    # TODO: Create method return_item()
    # Should:
    # - Set _is_borrowed to False
    # - Return True
    
    # TODO: Create __str__ method
    # Should return: "Title: {title}, ID: {item_id}"
    
    # TODO: Create @classmethod get_total_items()
    # Should return the total number of items created
    # Hint:
    # @classmethod
    # def get_total_items(cls):
    #     return cls.total_items
```
---

✅ Test Your Code:
```python
# Test file: test_level1.py

from library_item import LibraryItem

# Test basic creation
item1 = LibraryItem("Python Basics", "B001")
print(item1)  # Should print: Title: Python Basics, ID: B001
print(f"Title: {item1.title}")
print(f"ID: {item1.item_id}")
print(f"Is borrowed: {item1.is_borrowed}")

# Test borrowing
print(f"First borrow: {item1.borrow()}")  # Should return True
print(f"Is borrowed now: {item1.is_borrowed}")  # Should be True
print(f"Second borrow: {item1.borrow()}")  # Should return False (already borrowed)

# Test returning
item1.return_item()
print(f"Is borrowed after return: {item1.is_borrowed}")  # Should be False

# Test class variable
item2 = LibraryItem("Advanced Python", "B002")
print(f"Total items: {LibraryItem.get_total_items()}")  # Should be 2

# Test encapsulation - these should fail or be renamed:
# print(item1.__item_id)  # AttributeError
print(item1._LibraryItem__item_id)  # Name mangling - works but ugly!
```
---
Expected Output:

```
Title: Python Basics, ID: B001
Title: Python Basics
ID: B001
Is borrowed: False
First borrow: True
Is borrowed now: True
Second borrow: False
Is borrowed after return: False
Total items: 2
B001
```
---

# Level 2: Inheritance + Polymorphism
## Task 2.1: Create Specific Item Types
 **File:** items.py

```python
from library_item import LibraryItem

class Book(LibraryItem):
    """Represents a book in the library"""
    
    def __init__(self, title, item_id, author, pages):
        # TODO: Call parent __init__ using super()
        # TODO: Add author and pages as instance attributes
        pass
    
    def get_borrow_period(self):
        """Books can be borrowed for 14 days"""
        return 14
    
    # TODO: Override __str__ to include author
    # Format: "Book: {title} by {author}, ID: {item_id}"


class Magazine(LibraryItem):
    """Represents a magazine in the library"""
    
    def __init__(self, title, item_id, issue_number):
        # TODO: Call parent __init__
        # TODO: Add issue_number as instance attribute
        pass
    
    def get_borrow_period(self):
        """Magazines can be borrowed for 7 days"""
        return 7
    
    # TODO: Override __str__
    # Format: "Magazine: {title} Issue #{issue_number}, ID: {item_id}"


class DVD(LibraryItem):
    """Represents a DVD in the library"""
    
    def __init__(self, title, item_id, duration_minutes):
        # TODO: Call parent __init__
        # TODO: Add duration_minutes as instance attribute
        pass
    
    def get_borrow_period(self):
        """DVDs can be borrowed for 3 days"""
        return 3
    
    # TODO: Override __str__
    # Format: "DVD: {title} ({duration_minutes} min), ID: {item_id}"
```
---

Test your code:

```python
# Test file: test_level2.py

from items import Book, Magazine, DVD

# Create different items
book = Book("Python Crash Course", "B001", "Eric Matthes", 544)
magazine = Magazine("Wired", "M001", 202)
dvd = DVD("Learn OOP", "D001", 120)

# Test polymorphism - same interface, different behavior
items = [book, magazine, dvd]

print("="*50)
print("POLYMORPHISM DEMO")
print("="*50)

for item in items:
    print(item)
    print(f"  Borrow period: {item.get_borrow_period()} days")
    print(f"  Type: {type(item).__name__}")
    print()

# Test inheritance - all items are LibraryItems
from library_item import LibraryItem

print("Inheritance check:")
print(f"book is LibraryItem: {isinstance(book, LibraryItem)}")
print(f"book is Book: {isinstance(book, Book)}")
print(f"magazine is Book: {isinstance(magazine, Book)}")

# Test method resolution order
print(f"\nBook's MRO: {Book.__mro__}")
```
---
Expected Output:

```
==================================================
POLYMORPHISM DEMO
==================================================
Book: Python Crash Course by Eric Matthes, ID: B001
  Borrow period: 14 days
  Type: Book

Magazine: Wired Issue #202, ID: M001
  Borrow period: 7 days
  Type: Magazine

DVD: Learn OOP (120 min), ID: D001
  Borrow period: 3 days
  Type: DVD

Inheritance check:
book is LibraryItem: True
book is Book: True
magazine is Book: False

Book's MRO: (<class '__main__.Book'>, <class 'library_item.LibraryItem'>, <class 'object'>)
```
# Level 3: Abstract Classes
## Task 3.1: Create User Hierarchy
**File:** user.py

```python
from abc import ABC, abstractmethod

class User(ABC):
    """Abstract base class for library users"""
    
    def __init__(self, name, user_id):
        self.name = name
        self.user_id = user_id
        self._borrowed_items = []  # List of LibraryItem objects
    
    @abstractmethod
    def get_max_borrow_limit(self):
        """
        Each user type has different borrow limits
        This MUST be implemented by child classes
        """
        pass
    
    @abstractmethod
    def get_fine_rate(self):
        """
        Fine per day for late returns (in dollars)
        This MUST be implemented by child classes
        """
        pass
    
    def can_borrow_more(self):
        """Check if user can borrow more items"""
        # TODO: Return True if len(_borrowed_items) < max_borrow_limit
        pass
    
    def borrow_item(self, item):
        """
        Attempt to borrow an item
        
        Returns:
            bool: True if successful, False otherwise
        """
        # TODO: Check if user can borrow more (use can_borrow_more)
        # TODO: Try to borrow the item (call item.borrow())
        # TODO: If successful, add to _borrowed_items
        # TODO: Return True/False based on success
        pass
    
    def return_item(self, item):
        """
        Return a borrowed item
        
        Returns:
            bool: True if successful, False if item not in borrowed list
        """
        # TODO: Check if item in _borrowed_items
        # TODO: Call item.return_item()
        # TODO: Remove from _borrowed_items
        # TODO: Return True/False
        pass
    
    def get_borrowed_items(self):
        """Get list of currently borrowed items"""
        return self._borrowed_items.copy()  # Return copy to prevent external modification
    
    def __str__(self):
        return f"{self.__class__.__name__}: {self.name} (ID: {self.user_id})"


class Student(User):
    """Student user - can borrow up to 3 items"""
    
    # TODO: Implement get_max_borrow_limit() - return 3
    
    # TODO: Implement get_fine_rate() - return 0.50 (50 cents per day)
    

class Teacher(User):
    """Teacher user - can borrow up to 5 items"""
    
    # TODO: Implement get_max_borrow_limit() - return 5
    
    # TODO: Implement get_fine_rate() - return 0.25 (25 cents per day)


class Guest(User):
    """Guest user - can borrow only 1 item"""
    
    # TODO: Implement get_max_borrow_limit() - return 1
    
    # TODO: Implement get_fine_rate() - return 1.00 ($1 per day)
```
---
✅ Test Your Code:

```python
# Test file: test_level3.py

from user import Student, Teacher, Guest, User
from items import Book

# Test that you can't instantiate abstract class
try:
    user = User("Test", "U001")
    print("ERROR: Should not be able to create User instance!")
except TypeError as e:
    print(f"✓ Correct: {e}")

# Create different user types
student = Student("Alice", "S001")
teacher = Teacher("Bob", "T001")
guest = Guest("Charlie", "G001")

users = [student, teacher, guest]

print("\n" + "="*50)
print("USER TYPES")
print("="*50)

for user in users:
    print(f"{user}")
    print(f"  Max borrow limit: {user.get_max_borrow_limit()}")
    print(f"  Fine rate: ${user.get_fine_rate()}/day")
    print()

# Test borrowing logic
print("="*50)
print("BORROWING TEST")
print("="*50)

book1 = Book("Book 1", "B001", "Author 1", 100)
book2 = Book("Book 2", "B002", "Author 2", 200)
book3 = Book("Book 3", "B003", "Author 3", 300)
book4 = Book("Book 4", "B004", "Author 4", 400)

# Student borrows books
print(f"Student borrows book1: {student.borrow_item(book1)}")  # True
print(f"Student borrows book2: {student.borrow_item(book2)}")  # True
print(f"Student borrows book3: {student.borrow_item(book3)}")  # True
print(f"Student borrows book4: {student.borrow_item(book4)}")  # False (limit reached)

print(f"\nStudent has {len(student.get_borrowed_items())} books")

# Student returns a book
print(f"Student returns book1: {student.return_item(book1)}")  # True
print(f"Student has {len(student.get_borrowed_items())} books")
print(f"Student borrows book4: {student.borrow_item(book4)}")  # Now True

# Guest tries to borrow multiple
guest_book = Book("Guest Book", "B005", "Author", 150)
print(f"\nGuest borrows book: {guest.borrow_item(guest_book)}")  # True
print(f"Guest borrows book1: {guest.borrow_item(book1)}")  # False (limit 1)
```
---
Expected Output:
```
✓ Correct: Can't instantiate abstract class User with abstract methods get_fine_rate, get_max_borrow_limit

==================================================
USER TYPES
==================================================
Student: Alice (ID: S001)
  Max borrow limit: 3
  Fine rate: $0.5/day

Teacher: Bob (ID: T001)
  Max borrow limit: 5
  Fine rate: $0.25/day

Guest: Charlie (ID: G001)
  Max borrow limit: 1
  Fine rate: $1.0/day

==================================================
BORROWING TEST
==================================================
Student borrows book1: True
Student borrows book2: True
Student borrows book3: True
Student borrows book4: False

Student has 3 books
Student returns book1: True
Student has 2 books
Student borrows book4: True

Guest borrows book: True
Guest borrows book1: False
```
---
# Level 4: Advanced Features
## Task 4.1: Add Date Tracking
**File:** borrow_record.py

```python
from datetime import datetime, timedelta

class BorrowRecord:
    """Tracks a single borrow transaction"""
    
    def __init__(self, item, user, borrow_date=None):
        """
        Args:
            item: LibraryItem being borrowed
            user: User who is borrowing
            borrow_date: datetime object (defaults to now)
        """
        self.item = item
        self.user = user
        self.borrow_date = borrow_date or datetime.now()
        self.due_date = self.borrow_date + timedelta(days=item.get_borrow_period())
        self.return_date = None
    
    def mark_returned(self, return_date=None):
        """Mark item as returned"""
        self.return_date = return_date or datetime.now()
    
    def is_overdue(self):
        """Check if item is overdue"""
        # TODO: If not returned and current date > due_date, return True
        # TODO: If returned and return_date > due_date, return True
        # TODO: Otherwise return False
        pass
    
    def days_overdue(self):
        """Calculate number of days overdue"""
        # TODO: If not overdue, return 0
        # TODO: Calculate difference between due_date and return_date (or now)
        # TODO: Return number of days (as integer)
        pass
    
    def calculate_fine(self):
        """Calculate fine for late return"""
        # TODO: Get days overdue
        # TODO: Multiply by user's fine rate
        # TODO: Return fine amount (float)
        pass
    
    def __str__(self):
        status = "Returned" if self.return_date else "Borrowed"
        fine = f", Fine: ${self.calculate_fine():.2f}" if self.is_overdue() else ""
        return (f"{self.user.name} - {self.item.title}\n"
                f"  Borrowed: {self.borrow_date.strftime('%Y-%m-%d')}\n"
                f"  Due: {self.due_date.strftime('%Y-%m-%d')}\n"
                f"  Status: {status}{fine}")
```
---
Test your Code:
```python
# Test file: test_level4.py

from datetime import datetime, timedelta
from borrow_record import BorrowRecord
from items import Book
from user import Student

# Create test data
student = Student("Alice", "S001")
book = Book("Python Basics", "B001", "John Doe", 300)

# Test on-time return
print("="*50)
print("TEST 1: On-time return")
print("="*50)

borrow_date = datetime(2024, 1, 1)
record = BorrowRecord(book, student, borrow_date)
print(record)
print(f"Is overdue: {record.is_overdue()}")
print(f"Fine: ${record.calculate_fine():.2f}")

# Return on time
return_date = datetime(2024, 1, 10)  # 9 days later (due in 14)
record.mark_returned(return_date)
print(f"\nAfter return:")
print(f"Is overdue: {record.is_overdue()}")
print(f"Fine: ${record.calculate_fine():.2f}")

# Test late return
print("\n" + "="*50)
print("TEST 2: Late return")
print("="*50)

record2 = BorrowRecord(book, student, borrow_date)
return_date2 = datetime(2024, 1, 20)  # 19 days later (5 days late)
record2.mark_returned(return_date2)
print(record2)
print(f"Days overdue: {record2.days_overdue()}")
print(f"Fine: ${record2.calculate_fine():.2f}")  # 5 days * $0.50 = $2.50
```
---

# Level 5: Library Management
## Task 5.1: Create Library Class
**File:**  library.py

```python
from datetime import datetime
from borrow_record import BorrowRecord

class Library:
    """Main library management system"""
    
    def __init__(self, name):
        self.name = name
        self._items = {}  # item_id: LibraryItem
        self._users = {}  # user_id: User
        self._active_borrows = {}  # (user_id, item_id): BorrowRecord
        self._history = []  # All borrow records
    
    def add_item(self, item):
        """Add an item to the library"""
        # TODO: Add to self._items dictionary using item.item_id as key
        pass
    
    def register_user(self, user):
        """Register a new user"""
        # TODO: Add to self._users dictionary using user.user_id as key
        pass
    
    def find_item(self, item_id):
        """Find item by ID"""
        # TODO: Return item from _items, or None if not found
        pass
    
    def find_user(self, user_id):
        """Find user by ID"""
        # TODO: Return user from _users, or None if not found
        pass
    
    def borrow_item(self, user_id, item_id):
        """
        Process a borrow transaction
        
        Returns:
            tuple: (success: bool, message: str)
        """
        # TODO: Find user and item
        # TODO: Check if they exist
        # TODO: Try to borrow (user.borrow_item(item))
        # TODO: If successful, create BorrowRecord
        # TODO: Add to _active_borrows with key (user_id, item_id)
        # TODO: Return (True, "Success message") or (False, "Error message")
        pass
    
    def return_item(self, user_id, item_id):
        """
        Process a return transaction
        
        Returns:
            tuple: (success: bool, message: str, fine: float)
        """
        # TODO: Find the borrow record in _active_borrows
        # TODO: Mark as returned
        # TODO: Calculate fine
        # TODO: Remove from _active_borrows
        # TODO: Add to _history
        # TODO: Call user.return_item(item)
        # TODO: Return (True/False, message, fine_amount)
        pass
    
    def get_user_borrowed_items(self, user_id):
        """Get all items currently borrowed by a user"""
        # TODO: Filter _active_borrows for this user_id
        # TODO: Return list of items
        pass
    
    def get_overdue_items(self):
        """Get all overdue borrow records"""
        # TODO: Filter _active_borrows for overdue records
        # TODO: Return list of records
        pass
    
    def search_items(self, keyword):
        """Search for items by title"""
        # TODO: Search through _items for titles containing keyword
        # TODO: Return list of matching items
        pass
    
    def get_statistics(self):
        """Get library statistics"""
        total_items = len(self._items)
        total_users = len(self._users)
        currently_borrowed = len(self._active_borrows)
        available = total_items - currently_borrowed
        
        return {
            'total_items': total_items,
            'total_users': total_users,
            'currently_borrowed': currently_borrowed,
            'available': available
        }
    
    def __str__(self):
        stats = self.get_statistics()
        return (f"Library: {self.name}\n"
                f"  Items: {stats['total_items']} ({stats['available']} available)\n"
                f"  Users: {stats['total_users']}\n"
                f"  Currently borrowed: {stats['currently_borrowed']}")
```

Test your Code:

```python
# Test file: test_level5.py

from library import Library
from items import Book, Magazine, DVD
from user import Student, Teacher

# Create library
lib = Library("City Central Library")

# Add items
lib.add_item(Book("Python Crash Course", "B001", "Eric Matthes", 544))
lib.add_item(Book("Clean Code", "B002", "Robert Martin", 464))
lib.add_item(Magazine("Wired", "M001", 202))
lib.add_item(DVD("Python Tutorial", "D001", 180))

# Register users
lib.register_user(Student("Alice", "S001"))
lib.register_user(Teacher("Bob", "T001"))

# Show library status
print(lib)

# Borrow items
print("\n" + "="*50)
print("BORROWING")
print("="*50)

success, msg = lib.borrow_item("S001", "B001")
print(f"Alice borrows Python Crash Course: {msg}")

success, msg = lib.borrow_item("S001", "M001")
print(f"Alice borrows Wired: {msg}")

success, msg = lib.borrow_item("T001", "B001")  # Already borrowed
print(f"Bob tries to borrow Python Crash Course: {msg}")

# Check what Alice borrowed
print("\n" + "="*50)
print("ALICE'S BORROWED ITEMS")
print("="*50)

alice_items = lib.get_user_borrowed_items("S001")
for item in alice_items:
    print(f"  - {item}")

# Return item
print("\n" + "="*50)
print("RETURNING")
print("="*50)

success, msg, fine = lib.return_item("S001", "B001")
print(f"Alice returns Python Crash Course: {msg}")
print(f"Fine: ${fine:.2f}")

# Show updated status
print("\n" + lib)
```

# Level 6: Special Methods & Operators
## Task 6.1: Make Items Comparable
### Add these methods to the LibraryItem class:

```python
def __eq__(self, other):
    """Two items are equal if they have the same item_id"""
    # TODO: Check if other is LibraryItem instance
    # TODO: Compare item_ids
    pass

def __lt__(self, other):
    """Compare items by title alphabetically"""
    # TODO: Compare titles
    pass

def __hash__(self):
    """Make items hashable (can be used in sets/dicts)"""
    # TODO: Return hash of item_id
    pass
```
---
Test your code:
```python
# Test file: test_level6.py

from items import Book

# Create books
book1 = Book("Python", "B001", "Author A", 300)
book2 = Book("Java", "B002", "Author B", 400)
book3 = Book("Python", "B003", "Author C", 350)
book4 = Book("Python", "B001", "Author A", 300)  # Same as book1

# Test equality
print("Equality tests:")
print(f"book1 == book4: {book1 == book4}")  # True (same item_id)
print(f"book1 == book3: {book1 == book3}")  # False (different item_id)

# Test comparison
books = [book1, book2, book3]
sorted_books = sorted(books)

print("\nSorted by title:")
for book in sorted_books:
    print(f"  {book.title}")

# Test hashability (can use in sets)
book_set = {book1, book2, book3, book4}
print(f"\nUnique books in set: {len(book_set)}")  # Should be 3 (book1 and book4 are same)
```
# 🎯 Final Challenge: Complete System
## Task 7: Create Main Application
**File:**  main.py
```python
from library import Library
from items import Book, Magazine, DVD
from user import Student, Teacher, Guest
from datetime import datetime, timedelta

def main():
    # Initialize library
    lib = Library("City Central Library")
    
    # Add items
    print("Adding items to library...")
    lib.add_item(Book("Python Crash Course", "B001", "Eric Matthes", 544))
    lib.add_item(Book("Clean Code", "B002", "Robert Martin", 464))
    lib.add_item(Book("The Pragmatic Programmer", "B003", "Hunt & Thomas", 352))
    lib.add_item(Magazine("National Geographic", "M001", 450))
    lib.add_item(Magazine("Wired", "M002", 202))
    lib.add_item(DVD("Python Tutorial", "D001", 180))
    lib.add_item(DVD("Clean Code Video", "D002", 240))
    
    # Register users
    print("Registering users...")
    lib.register_user(Student("Alice Johnson", "S001"))
    lib.register_user(Student("Bob Smith", "S002"))
    lib.register_user(Teacher("Dr. Carol White", "T001"))
    lib.register_user(Guest("David Brown", "G001"))
    
    # Display library status
    print("\n" + "="*60)
    print(lib)
    print("="*60)
    
    # Simulate borrowing
    print("\n📚 BORROWING TRANSACTIONS:")
    transactions = [
        ("S001", "B001"),
        ("S001", "M001"),
        ("T001", "B002"),
        ("T001", "B003"),
        ("G001", "D001"),
        ("S002", "B001"),  # Should fail - already borrowed
        ("G001", "M002"),  # Should fail - guest limit reached
    ]
    
    for user_id, item_id in transactions:
        success, msg = lib.borrow_item(user_id, item_id)
        status = "✓" if success else "✗"
        print(f"{status} {msg}")
    
    # Show borrowed items per user
    print("\n📖 CURRENTLY BORROWED:")
    for user_id in ["S001", "T001", "G001"]:
        user = lib.find_user(user_id)
        items = lib.get_user_borrowed_items(user_id)
        print(f"\n{user.name}:")
        for item in items:
            print(f"  - {item.title}")
    
    # Simulate returns
    print("\n" + "="*60)
    print("📥 RETURN TRANSACTIONS:")
    print("="*60)
    
    returns = [
        ("S001", "B001"),
        ("T001", "B002"),
    ]
    
    for user_id, item_id in returns:
        success, msg, fine = lib.return_item(user_id, item_id)
        print(f"✓ {msg}")
        if fine > 0:
            print(f"   Fine: ${fine:.2f}")
    
    # Search functionality
    print("\n" + "="*60)
    print("🔍 SEARCH: 'Python'")
    print("="*60)
    
    results = lib.search_items("Python")
    for item in results:
        print(f"  - {item}")
    
    # Final statistics
    print("\n" + "="*60)
    print("📊 FINAL STATISTICS")
    print("="*60)
    print(lib)
    
    # Show overdue items (if any)
    overdue = lib.get_overdue_items()
    if overdue:
        print("\n⚠️  OVERDUE ITEMS:")
        for record in overdue:
            print(f"  - {record}")
    else:
        print("\n✓ No overdue items!")

if __name__ == "__main__":
    main()
```
---
# 📚 Extension Challenges (Optional)
## Once you complete the main project, try these:

### Challenge 1: Persistence
### Add ability to save/load library state to JSON file

```python
import json

class Library:
    def save_to_file(self, filename):
        # TODO: Convert library data to JSON
        pass
    
    @classmethod
    def load_from_file(cls, filename):
        # TODO: Load library from JSON
        pass
```
---

## Challenge 2: GUI (tkinter)

Build a simple desktop interface to browse items, register users, borrow/return, and view overdue items.

Suggested components
- Main window: library name, stats
- Item list view: search box, filter by type, select item
- User panel: register/login, view borrowed items
- Actions: Borrow, Return, Reserve
- Notifications area: success/error messages

Minimal tkinter skeleton
```python
import tkinter as tk
from tkinter import ttk

def main():
    root = tk.Tk()
    root.title("Library Management")
    root.geometry("800x600")

    header = ttk.Label(root, text="City Central Library", font=("Segoe UI", 16))
    header.pack(pady=8)

    # TODO: add listbox/treeview, forms, buttons and bind actions to Library methods

    root.mainloop()

if __name__ == "__main__":
    main()
```

Tips
- Keep GUI logic separate from core library classes (use controllers).
- Use ttk.Treeview for tabular lists.
- Use dialogs for confirmations and forms.

---

## Challenge 3: Database (SQLite)

Persist items, users, and borrow records using SQLite.

Suggested schema
- items(item_id TEXT PRIMARY KEY, title TEXT, type TEXT, meta JSON)
- users(user_id TEXT PRIMARY KEY, name TEXT, role TEXT, password_hash TEXT)
- borrows(id INTEGER PRIMARY KEY AUTOINCREMENT, user_id TEXT, item_id TEXT, borrow_date TEXT, due_date TEXT, return_date TEXT, FOREIGN KEYS...)

Integration notes
- Provide a small Data Access Layer (DAL) with methods: add_item, get_item, add_user, borrow_item, return_item, list_overdues.
- Use sqlite3 or SQLAlchemy for convenience.
- Store item-specific fields (author, pages) in a JSON column or separate tables.

Migrations
- Create tables at startup if not present.
- Keep simple versioning (a schema_version table) if evolving schema.

---

## Challenge 4: Authentication

Add password protection for users and basic session handling.

Recommendations
- Never store plain passwords. Hash using bcrypt (preferred) or hashlib.pbkdf2_hmac.
- Registration: collect password, store hash + salt.
- Login: verify password, create an in-memory session token (or a simple logged-in user object).
- Optional: role-based access (Student, Teacher, Guest) to limit operations.

Example using bcrypt
```python
import bcrypt

pw_hash = bcrypt.hashpw(password.encode(), bcrypt.gensalt())
bcrypt.checkpw(candidate.encode(), pw_hash)
```

Security tips
- Use a proper password policy (min length).
- If using GUI, avoid storing passwords in plain text files.

---

## Challenge 5: Reservation System

Allow users to reserve currently borrowed items.

Data model
- reservations(id, user_id, item_id, reserved_at, status) — status: pending/fulfilled/cancelled
- Each item can have a queue of reservations.

Behavior
- When an item is returned, check reservation queue and notify the next user.
- A reservation expires after N days if not claimed.
- Prevent users from reserving an item they already borrowed or currently have reserved.

API suggestions
- Library.reserve_item(user_id, item_id) -> (bool, message)
- Library.cancel_reservation(user_id, item_id)
- Library._process_reservation_on_return(item_id)

---

## Challenge 6: Email Notifications (smtplib)

Send email reminders for upcoming due dates and when reserved items become available.

Basic flow
- Configure SMTP settings (host, port, TLS, credentials) — use environment variables or config file.
- Compose plain-text (or simple HTML) messages.
- Schedule reminder jobs (e.g., 3 days before due date, on due date, overdue alerts).

Example using smtplib
```python
import smtplib
from email.message import EmailMessage

msg = EmailMessage()
msg["Subject"] = "Library Reminder"
msg["From"] = "library@example.com"
msg["To"] = "user@example.com"
msg.set_content("Your book is due soon.")

with smtplib.SMTP("smtp.example.com", 587) as s:
    s.starttls()
    s.login(user, password)
    s.send_message(msg)
```

Scheduling options
- For simple apps: use threading.Timer or background thread with sleep.
- For production-like scheduling: use APScheduler or cron.

Security note
- Use app-specific passwords or OAuth for providers like Gmail.
- Don’t hardcode credentials in source.

---

✅ Solution Checklist
- [ ] All classes documented with docstrings
- [ ] All TODOs implemented
- [ ] All test files pass
- [ ] Encapsulation respected (private/protected attributes)
- [ ] Inheritance hierarchy correct
- [ ] Polymorphism works (borrow periods)
- [ ] Abstract methods implemented in children
- [ ] @property used for getters
- [ ] Special methods (__str__, __eq__, __lt__, __hash__) implemented
- [ ] Late fees calculated correctly
- [ ] Borrow limits enforced
- [ ] Items cannot be borrowed twice
- [ ] main.py runs without errors
- [ ] Optional: GUI, DB persistence, authentication, reservations, email notifications implemented

---

🎓 Key Takeaways
- Separate UI, persistence, and business logic
- Prefer small, testable units (DAL, service layer)
- Use secure password handling and safe credential storage
- Design models for extensibility (reservations, fines, notifications)

---

💡 Tips for Success
- Implement incrementally and run tests frequently
- Keep interfaces (Library class) stable while swapping implementations (in-memory vs DB)
- Log operations for easier debugging
- Write unit tests for edge cases (concurrent reservations, double-borrow attempts)

---

🚀 Next Steps
- Add REST API (Flask/FastAPI) to expose library operations
- Add role-based web UI and admin panel
- Integrate background job worker (Redis + RQ) for email/scheduling
- Add analytics: popular items, frequent borrowers, revenue from fines
- Containerize app (Docker) and add simple CI tests

