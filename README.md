import java.util.*;

// Book class
class Book {
    private String title;
    private String author;
    private boolean isIssued;

    public Book(String title, String author) {
        this.title = title;
        this.author = author;
        this.isIssued = false;
    }

    public String getTitle() {
        return title;
    }

    public boolean isIssued() {
        return isIssued;
    }

    public void issueBook() {
        isIssued = true;
    }

    public void returnBook() {
        isIssued = false;
    }

    @Override
    public String toString() {
        return title + " by " + author + (isIssued ? " [Issued]" : " [Available]");
    }
}

// Abstract User class (Abstraction)
abstract class User {
    protected String name;
    protected int userId;

    public User(String name, int userId) {
        this.name = name;
        this.userId = userId;
    }

    public abstract void showInfo();  // Polymorphism (method to override in subclass)

    public String getName() {
        return name;
    }

    public int getUserId() {
        return userId;
    }
}

// Student class (Inheritance from User)
class Student extends User {
    public Student(String name, int userId) {
        super(name, userId);
    }

    @Override
    public void showInfo() {
        System.out.println("Student Name: " + name + ", ID: " + userId);
    }
}

// Library class
class Library {
    private List<Book> books;
    private Map<Integer, List<Book>> issuedBooks;

    public Library() {
        books = new ArrayList<>();
        issuedBooks = new HashMap<>();
    }

    public void addBook(Book book) {
        books.add(book);
    }

    public void displayBooks() {
        for (Book b : books) {
            System.out.println(b);
        }
    }

    public void issueBook(String title, User user) {
        for (Book book : books) {
            if (book.getTitle().equalsIgnoreCase(title) && !book.isIssued()) {
                book.issueBook();
                issuedBooks.computeIfAbsent(user.getUserId(), k -> new ArrayList<>()).add(book);
                System.out.println("Book issued to " + user.getName());
                return;
            }
        }
        System.out.println("Book not available.");
    }

    public void returnBook(String title, User user) {
        List<Book> userBooks = issuedBooks.get(user.getUserId());
        if (userBooks != null) {
            for (Book book : userBooks) {
                if (book.getTitle().equalsIgnoreCase(title)) {
                    book.returnBook();
                    userBooks.remove(book);
                    System.out.println("Book returned by " + user.getName());
                    return;
                }
            }
        }
        System.out.println("No such book issued to user.");
    }
}
public class LibrarySystem {
    public static void main(String[] args) {
        Library library = new Library();

        // Add some books
        library.addBook(new Book("Java Basics", "James Gosling"));
        library.addBook(new Book("Python Fundamentals", "Guido van Rossum"));

        // Create users
        User student1 = new Student("Alice", 101);
        User student2 = new Student("Bob", 102);

        // Display books
        System.out.println("Available books:");
        library.displayBooks();

        // Issue a book
        library.issueBook("Java Basics", student1);

        // Try to issue the same book again
        library.issueBook("Java Basics", student2);

        // Return a book
        library.returnBook("Java Basics", student1);

        // Display books after return
        System.out.println("\nBooks after return:");
        library.displayBooks();
    }
}
