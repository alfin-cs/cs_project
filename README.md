```
import mysql.connector as m
db = m.connect(host="localhost", user="root", password="alfin-ashwin")
c = db.cursor()
c.execute("CREATE DATABASE IF NOT EXISTS library")
c.execute("USE library")
c.execute("""CREATE TABLE IF NOT EXISTS books (
        bk_id INT PRIMARY KEY,
        bk_name VARCHAR(255),
        author VARCHAR(255),
        genre VARCHAR(100),
        status VARCHAR(20))""")
c.execute("""CREATE TABLE IF NOT EXISTS user (
        bk_id INT PRIMARY KEY,
        bk_name VARCHAR(255),
        author VARCHAR(255),
        genre VARCHAR(100),
        status VARCHAR(20))""")
c.execute("delete from books")
c.execute("delete from user")
c.execute("""
INSERT INTO books VALUES
(1, 'To Kill a Mockingbird', 'Harper Lee', 'Fiction', 'available'),
(2, '1984', 'George Orwell', 'Dystopian', 'available'),
(3, 'The Great Gatsby', 'F. Scott Fitzgerald', 'Classic', 'available'),
(4, 'Harry Potter and the Sorcerer''s Stone', 'J.K. Rowling', 'Fantasy', 'available'),
(5, 'The Hobbit', 'J.R.R. Tolkien', 'Fantasy', 'available'),
(6, 'Pride and Prejudice', 'Jane Austen', 'Romance', 'available'),
(7, 'The Catcher in the Rye', 'J.D. Salinger', 'Fiction', 'available'),
(8, 'Brave New World', 'Aldous Huxley', 'Dystopian', 'available'),
(9, 'The Chronicles of Narnia', 'C.S. Lewis', 'Fantasy', 'available'),
(10, 'Moby Dick', 'Herman Melville', 'Adventure', 'available'),
(11, 'The Alchemist', 'Paulo Coelho', 'Fiction', 'available'),
(12, 'War and Peace', 'Leo Tolstoy', 'Historical', 'available'),
(13, 'Crime and Punishment', 'Fyodor Dostoevsky', 'Crime', 'available'),
(14, 'Jane Eyre', 'Charlotte Bronte', 'Gothic', 'available'),
(15, 'The Lord of the Rings', 'J.R.R. Tolkien', 'Fantasy', 'available')
""")
db.commit()
print("Database, tables, and initial records created successfully.")



def user_menu():
    while True:
        print("="*40)
        print(" "*8 + "LIBRARY MANAGEMENT SYSTEM")
        print("="*40)
        print("\nPlease choose an option from the menu below:\n")
        print(" 1. View Book Catalog")
        print(" 2. Borrow a Book")
        print(" 3. Return a Book")
        print(" 4. Enter as Admin")
        print(" 5. Exit")
        print("="*40)
        try:        
            ch_menu =int(input("Enter your choice (1-5):"))
        except ValueError:
            print("Invalid input! please enter a number between 1 and 5.\n")
        print("="*40)
        if ch_menu==1:
            bookcatalog()
        elif ch_menu==2:
            borrow_book()
        elif ch_menu==3:
            returnbook()
        elif ch_menu==4:
            for i in range(1,4): 
                password=input("Enter Password: ")
                if password =="adminmode":
                    admin()
                    break
                else:
                    print("Incorrect password You have",3-i,"more trys")
            print("Returning to menu")
            continue
        elif ch_menu==5:
            print("Thank You")
            break
        else:
            print("Invalid choice. Please enter a number between 1 and 5.\n")
        print("="*40)
        
def bookcatalog():
    while True:
        print("=" * 40)
        print(" " * 12 + "BOOK CATALOG")
        print("=" * 40)
        print(" 1. See all available Books")
        print(" 2. Search by Genre")
        print(" 3. Search for a Book")
        print(" 4. Borrowed Catalog")
        print(" 5. Exit")
        print("=" * 40)
        try:
            ch_catalog = int(input("Enter your choice (1-5): "))
        except ValueError:
            print("Invalid input! Please enter a number between 1 and 4.\n")
            continue
        print("=" * 40)
        if ch_catalog == 1:
            print("=" * 40)
            print(" " * 9 + "ALL AVAILABLE BOOKS")
            print("=" * 40)
            c.execute("select * FROM books")
            s_all = c.fetchall()
            for i in s_all:
                print(i)
                print("-" * 40)
        elif ch_catalog == 2:
            genre = input("Enter your preferred genre: ")
            print("=" * 40)
            print(" " * 9 + "BOOKS BY GENRE")
            print("=" * 40)
            c.execute("select * from books where genre='{}'".format(genre))
            s_genre = c.fetchall()
            if s_genre:
                for i in s_genre:
                    print(i)
                    print("-" * 40)
            else:
                print("No books found in the selected genre")
        elif ch_catalog == 3:
            while True:
                print("=" * 40)
                print(" " * 10 + "SEARCH BOOK")
                print("=" * 40)
                print(" 1. By Book Name")
                print(" 2. By Book ID")
                print(" 3. Back to Catalog")
                print("=" * 40)
                try:
                    choice = int(input("Enter your choice (1-3): "))
                except ValueError:
                    print("Invalid input! Please enter a number between 1 and 3.\n")
                    continue
                if choice == 1:
                    na_choice = input("Enter Book Name: ")
                    try:
                        c.execute("select * from books where bk_name = '{}'".format(na_choice))
                        results = c.fetchall()
                        if results:
                            print("=" * 40)
                            for i in results:
                                print(i)
                                print("-" * 40)
                        else:
                            print("Book not found or not available.\n")
                    except Exception as e:
                        print("Error:", e)
                elif choice == 2:
                    try:
                        id_choice = int(input("Enter Book ID: "))
                        c.execute("select * from books where bk_id = {}".format(id_choice))
                        results = c.fetchall()
                        if results:
                            print("=" * 40)
                            for i in results:
                                print(i)
                                print("-" * 40)
                        else:
                            print("Book not found or not available.\n")
                    except Exception as e:
                        print("Error:", e)
                elif choice == 3:
                    break
                else:
                    print("Invalid input! Please enter a number between 1 and 3.\n")
        elif ch_catalog == 4:
            print("=" * 40)
            print(" " * 8 + "BORROWED CATALOG")
            print("=" * 40)
        
            c.execute("SELECT * FROM user")
            book3 = c.fetchone()
        
            if book3:
                c.execute("SELECT * FROM user")
                s_borrow = c.fetchall()
                for i in s_borrow:
                    print(i)
                    print("-" * 40)
            else:
                print("No books have been borrowed yet.\n")
        elif ch_catalog == 5:
            user_menu()
            break
        else:
            print("Invalid choice. Please enter a number between 1 and 4.\n")
def borrow_book():
    while True:
        print("=" * 40)
        print(" " * 8 + "BORROW BOOK" + " " * 10)
        print("=" * 40)
        print("How do you want to borrow?")
        print(" 1. By Book Name")
        print(" 2. By Book ID")
        print(" 3. Exit")
        print("=" * 40)
        try:
            choice = int(input("Enter your choice (1-3): "))
            print("=" * 40)
        except ValueError:
            print("Invalid input! Please enter a number between 1 and 3.\n")
            continue  
        if choice == 1:
            try:
                na_choice = input("Enter Book Name: ")
                c.execute("select * from books where bk_name = '{}' and status = 'available'".format(na_choice))
                book1 = c.fetchone()
                if book1:
                    c.execute("insert into user select * from books where bk_name='{}'".format(na_choice))
                    c.execute("update books set status='borrowed' where bk_name='{}'".format(na_choice))
                    c.execute("update user set status='Checked Out' where bk_name='{}'".format(na_choice))
                    db.commit()
                    print("Book '{}' has been borrowed.\n".format(na_choice))
                else:
                    print("Book not found or not available.\n")
                    continue
            except :
                print("An error ocurred")
                continue
        elif choice == 2:
            try:
                id_choice = int(input("Enter Book ID: "))
                c.execute("select * from books where bk_id = {} and status = 'available'".format(id_choice))
                book2 = c.fetchone()
                if book2:
                    c.execute("insert into user select * from books where bk_id={}".format(id_choice))
                    c.execute("update books set status='borrowed' where bk_id={}".format(id_choice))
                    db.commit()
                    print("Book ID {} has been borrowed.\n".format(id_choice))
                else:
                    print("Book not found or not available.\n")
                    continue
            except ValueError:
                print("Invalid Book ID. Please enter a valid number.")
                continue
        elif choice == 3:
            user_menu()
            break  
        else:
            print("Invalid choice. Please enter a number between 1 and 3.\n")
def returnbook():
    while True:
        print("=" * 40)
        print(" " * 8 + "RETURN BOOK" + " " * 10)
        print("=" * 40)
        print("How do you want to return?")
        print(" 1. By Book Name")
        print(" 2. By Book ID")
        print(" 3. Exit")
        print("=" * 40)
        try:
            choice = int(input("Enter your choice (1-3): "))
            print("=" * 40)
        except ValueError:
            print("Invalid input! Please enter a number between 1 and 3.\n")
            continue  
        if choice == 1:
            try:
                na_choice = input("Enter Book Name: ")
                c.execute("select * from user where bk_name = '{}'".format(na_choice))
                book1 = c.fetchone()
                if book1:
                    c.execute("delete from user where bk_name='{}'".format(na_choice))
                    c.execute("update books set status='available' where bk_name='{}'".format(na_choice))
                    db.commit()
                    print("Book '{}' has been returned.\n".format(na_choice))   
                else:
                    print("Book not found in borrowed catalog.\n")
                    continue
            except :
                print("An error ocurred")
                continue
        elif choice == 2:
            try:
                id_choice = int(input("Enter Book ID: "))
                c.execute("select * from user where bk_id = {}".format(id_choice))
                book2 = c.fetchone()
                if book2:
                    c.execute("delete from user where bk_id={}".format(id_choice))
                    c.execute("update books set status='available' where bk_id={}".format(id_choice))
                    db.commit()
                    print("Book ID {} has been returned.\n".format(id_choice))   
                else:
                    print("Book not found in borrowed catalog.\n")
                    continue
            except ValueError:
                print("Invalid Book ID. Please enter a valid number.")
                continue
        elif choice == 3:
            user_menu()
            break

def admin():
    while True:
        print("=" * 40)
        print(" " * 8 + "ADMIN MODE")
        print("=" * 40)
        print(" 1. Add New Records")
        print(" 2. Change Record")
        print(" 3. Delete Record")
        print(" 4. Return to User Menu")
        print(" 5. Exit")
        print("=" * 40)
        try:
            ad_menu = int(input("Enter your choice (1-5): "))
        except ValueError:
            print("Invalid input! Please enter a number between 1 and 5.\n")
            continue
        print("=" * 40)
        if ad_menu == 1:
            add()
        elif ad_menu == 2:
            change()
        elif ad_menu == 3:
            delete()
        elif ad_menu == 4:
            user_menu()
        elif ad_menu == 5:
            break
        else:
            print("Please enter a valid choice (1-5).")
def add():
    while True:
        print("=" * 40)
        print(" " * 10 + "ADD NEW BOOK")
        print("=" * 40)
        try:
            id = int(input("Enter Book ID: "))
        except ValueError:
            print("Invalid Book ID! Please enter a number.")
            continue
        name = input("Enter Book Name: ")
        author = input("Enter Author Name: ")
        genre = input("Enter Book Genre: ")
        print("=" * 40)
        print("Entered Book Details:")
        print("-" * 40)
        print("Book ID: {}".format(id))
        print("Name: {}".format(name))
        print("Author: {}".format(author))
        print("Genre: {}".format(genre))
        
        add_ch = input("Are you sure you want to add this book? (Y/N): ")
        print("="*40)
        if add_ch in 'Yy':
            c.execute("Select * from books where bk_id={}".format(id))
            book=c.fetchone()
            if book:
                print("A record with this book id already exist")
                continue
            else:
                try:
                    c.execute("insert into books values({}, '{}', '{}', '{}','available')".format(id, name, author, genre))
                    db.commit()
                    print("Book added successfully.\n")
                except Exception as e:
                    print("Error:", e)
        elif add_ch in 'Nn':
            print("Book entry discarded.\n")
            break
        else:
            print("Invalid choice. Please enter 'Y' or 'N'.\n")
        cont = input("Do you want to add another book? (Y/N): ")
        if cont in 'Nn':
            break

def change():
    while True:
        print("=" * 40)
        print(" " * 10 + "Change Record")
        print("=" * 40)
        print("Select a record to change by")
        print(" 1. By Book Name")
        print(" 2. By Book ID")
        print(" 3. Exit")
        print("=" * 40)
        try:
            choice = int(input("Enter your choice (1-3): "))
        except ValueError:
            print("Invalid input! Please enter a number between 1 and 3.\n")
            continue  
        print("=" * 40)
        if choice == 1:
            try:
                na_choice = input("Enter Book Name: ")
                c.execute("select * from books where bk_name = '{}'".format(na_choice))
                book1 = c.fetchone()
                if book1:
                    print("=" * 40)
                    try:
                        new_id=int(input("Enter New Book ID: "))
                    except ValueError:
                        print("Invalid Book ID! Please enter a number.")
                        continue
                    new_name=input("Enter New Book Name: ")
                    new_author=input("Enter New Author: ")
                    new_genre=input("Enter Corrected genre")
                    a = input("Enter New Status (A = Available, B = Borrowed): ")
                    if a in "Aa":
                        new_status = "available"
                    elif a in "Bb":
                        new_status = "borrowed"
                    else:
                        print("Invalid status input. Must be 'A' or 'B'.")
                        continue
                 
                    try:
                        c.execute("UPDATE books SET bk_id={}, bk_name='{}', author='{}', genre='{}', status='{}' WHERE bk_name='{}'".format(
                            new_id, new_name, new_author, new_genre, new_status,new_name))
                        db.commit()
                        print("Book record updated successfully.\n")
                    except Exception as e:
                        print("Error:", e)
                else:
                    print("Book not found.\n")
                    continue
            except Exception as e:
                print("Error:", e)
                continue
        elif choice == 2:
            try:
                id_choice = int(input("Enter Book ID: "))
                c.execute("select * from books where bk_id = {} and status = 'available'".format(id_choice))
                book2 = c.fetchone()
                if book2:
                    print("=" * 40)
                    try:
                        new_id=int(input("Enter New BookID: "))
                    except ValueError:
                        print("Invalid Book ID! Please enter a number.")
                        continue
                    new_name=input("Enter New Book Name: ")
                    new_author=input("Enter New Author: ")
                    new_genre=input("Enter Corrected genre")
                    a = input("Enter New Status (A = Available, B = Borrowed): ")
                    if a in "Aa":
                        new_status = "available"
                    elif a in "Bb":
                        new_status = "borrowed"
                    else:
                        print("Invalid status input. Must be 'A' or 'B'.")
                        return
                    try:
                        c.execute("UPDATE books SET bk_id={}, bk_name='{}', author='{}', genre='{}', status='{}' WHERE bk_id={}".format(
                            new_id, new_name, new_author, new_genre, new_status,new_id))
                        db.commit()
                        print("Book record updated successfully.\n")
                    except Exception as e:
                        print("Error:", e)
            except ValueError:
                print("Invalid Book ID. Please enter a valid number.")
                continue
        elif choice == 3:
            break  
        else:
            print("Invalid choice. Please enter a number between 1 and 3.\n")

def delete():
    while True:
        print("="*40)
        print(" " * 8 + "DELETE RECORD" + " " * 10)
        print("="*40)
        print("Select a record to delete by")
        print(" 1. By Book Name")
        print(" 2. By Book ID")
        print(" 3. Exit")
        print("="*40)
        try:
            choice=int(input("Enter your choice (1-3): "))
            print("="*40)
        except ValueError:
            print("Invalid input! Please enter a number between 1 and 3.\n")
            continue
        if choice == 1:
            try:
                na_choice = input("Enter Book Name: ")
                c.execute("select * from books where bk_name = '{}'".format(na_choice))
                book1 = c.fetchone()
                if book1:
                    c.execute("delete from books where bk_name='{}'".format(na_choice))
                    db.commit()
                    print("Book '{}' has been deleted.\n".format(na_choice))
                else:
                    print("Book not found.\n")
                    continue
            except :
                print("An error occurred:")
                continue
        elif choice==2:
            try:
                id_choice = int(input("Enter Book ID: "))
                c.execute("select * from books where bk_id = {}".format(id_choice))
                book2 = c.fetchone()
                if book2:
                    c.execute("delete from books where bk_id={}".format(id_choice))
                    db.commit()
                    print("Book ID {} has been deleted.\n".format(id_choice))
                else:
                    print("Book not found.\n")
                    continue
            except ValueError:
                print("Invalid Book ID. Please enter a valid number.")
                continue
        elif choice == 3:
            break
        else:
            print("Invalid choice. Please enter a number between 1 and 3.\n")                                                                   
user_menu()
db.close()
```
