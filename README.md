// Lauren Acosta, CEN 3024C, 1/22/2024
// class name: Main
/* Overall program objective : My LMS program showcases the operations in a library management system, including adding, removing, and listing books.
 It consists of two main classes: Book and BookCatalog
 */
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

// Lauren Acosta, CEN 3024C, 1/22/2024
// Class name: Book
//'Book' is a representation of a book entity in the LMS. It encapsulates the information related to a book such as its
// attributes ( bookID, title, author). It provides a way to initialize its state, and offers methods to retrieve information about the book.
 
public class Book
{
    private int bookID;
    private String title;
    private String author;

    // constructor for initializing the attributes of a Book object when it is created.
    public Book(int bookID, String title, String author) {
        this.bookID = bookID;
        this.title = title;
        this.author = author;
    }

    //Getter methods (getBookID(), getTitle(), getAuthor()) allow other classes to access the values of the private attributes
    public int getBookID() {

        return bookID;
    }

    // Getter for title
    public String getTitle() {

        return title;
    }

    // Getter for author
    public String getAuthor() {

        return author;
    }

    // Method name: toString
    // returns a string containing the book's ID, title, and author in a formatted manner for user output.
    @Override
    public String toString() {
        return "Book{" +
                "bookID=" + bookID +
                ", title='" + title + '\'' +
                ", author='" + author + '\'' +
                '}';
    }
}
// Lauren Acosta, CEN 3024C, 1/22/2024
// Class name: BookCatalog
//'BookCatalog' is responsible for managing a collection of Book objects in the LMS.
// It provides methods for adding books from a file, removing books by ID, and listing all books in the catalog.
public class BookCatalog
{
    private List<Book> books;

    // constructor of BookCatalog (public BookCatalog()) is responsible for initializing the books list.
    // It creates an empty list when a BookCatalog object is instantiated.
    public BookCatalog() {
        this.books = new ArrayList<>();
    }

    // Method name: addBookFromFile
    // Reads books from a text file specified by filePath and adds them to both the books list and a new list (newBooks).
    // Returns the list of newly added books.
    public List<Book> addBookFromFile(String filePath) {
        List<Book> newBooks = new ArrayList<>();

        try (BufferedReader br = new BufferedReader(new FileReader(filePath))) {
            String line;

            while ((line = br.readLine()) != null)
            {
                String[] bookInfo = line.split(",");
                int bookID = Integer.parseInt(bookInfo[0]);
                String title = bookInfo[1];
                String author = bookInfo[2];
                Book newBook = new Book(bookID, title, author);
                newBooks.add(newBook);
                books.add(newBook);
            }
        }
        catch (IOException e) {
            e.printStackTrace();
        }
        return newBooks;
    }

    // Method name: removeBookByID
    // Removes a book from the books list based on its ID (bookID).
    // Returns true if the removal is successful and false otherwise.
    public boolean removeBookByID(int bookID) {
        Iterator<Book> iterator = books.iterator();
        while (((Iterator<?>) iterator).hasNext()) {
            Book book = iterator.next();
            if (book.getBookID() == bookID) {
                iterator.remove();
                return true;
            }
        }
        return false;
    }

    // Method name: listAllBooks
    // Lists all books in the collection
    // Returns a new List containing all the books in the books list.
    // It ensures that external classes get a copy of the book collection.
    public List<Book> listAllBooks() {
        return new ArrayList<>(books);
    }


}

public class Main {
    public static void main(String[] args)
    {

        // Display a welcome message
        System.out.println("Hello, Welcome to the LMS!");
        // Instantiate BookCatalog
        BookCatalog catalog = new BookCatalog();

        // Add books from a file
        List<Book> addedBooks = catalog.addBookFromFile("LMS_in.txt");
        System.out.println("Added Books: " + addedBooks);

        // List all books
        List<Book> allBooks = catalog.listAllBooks();
        System.out.println("All Books: " + allBooks);

        // Remove a book by ID
        boolean removed = catalog.removeBookByID(4248);
        System.out.println("Book Removed: " + removed);

        // List all books after removal
        allBooks = catalog.listAllBooks();
        System.out.println("All Books after Removal: " + allBooks);

    }
}

