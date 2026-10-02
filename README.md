import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

class Book {
    private final int id;
    private final String title;
    private final String author;
    private boolean available;
    private String issuedTo;

    public Book(int id, String title, String author) {
        this.id = id;
        this.title = title;
        this.author = author;
        this.available = true;
        this.issuedTo = null;
    }

    public int getId() {
        return id;
    }

    public String getTitle() {
        return title;
    }

    public String getAuthor() {
        return author;
    }

    public boolean isAvailable() {
        return available;
    }

    public String getIssuedTo() {
        return issuedTo;
    }

    public void issueTo(String memberName) {
        if (!available) {
            throw new IllegalStateException("Book is already issued.");
        }
        available = false;
        issuedTo = memberName;
    }

    public void returnBook() {
        available = true;
        issuedTo = null;
    }

    @Override
    public String toString() {
        return "ID: " + id +
                " | Title: " + title +
                " | Author: " + author +
                " | Status: " + (available ? "Available" : "Issued to " + issuedTo);
    }
}

class Library {
    private final List<Book> books = new ArrayList<>();
    private int nextBookId = 1;

    public void addBook(String title, String author) {
        if (title == null || title.trim().isEmpty()) {
            throw new IllegalArgumentException("Book title cannot be empty.");
        }
        if (author == null || author.trim().isEmpty()) {
            throw new IllegalArgumentException("Author name cannot be empty.");
        }

        books.add(new Book(nextBookId++, title.trim(), author.trim()));
        System.out.println("Book added successfully.");
    }

    public void listBooks() {
        if (books.isEmpty()) {
            System.out.println("No books available.");
            return;
        }

        for (Book book : books) {
            System.out.println(book);
        }
    }

    public Book findBookById(int id) {
        for (Book book : books) {
            if (book.getId() == id) {
                return book;
            }
        }
        return null;
    }

    public void issueBook(int bookId, String memberName) {
        Book book = findBookById(bookId);
        if (book == null) {
            System.out.println("Book not found.");
            return;
        }

        if (memberName == null || memberName.trim().isEmpty()) {
            System.out.println("Member name is required.");
            return;
        }

        try {
            book.issueTo(memberName.trim());
            System.out.println("Book issued successfully.");
        } catch (IllegalStateException e) {
            System.out.println(e.getMessage());
        }
    }

    public void returnBook(int bookId) {
        Book book = findBookById(bookId);
        if (book == null) {
            System.out.println("Book not found.");
            return;
        }

        if (book.isAvailable()) {
            System.out.println("This book is already available.");
            return;
        }

        book.returnBook();
        System.out.println("Book returned successfully.");
    }
}

public class LibraryManagementSystem {
    public static void main(String[] args) {
        Library library = new Library();
        Scanner scanner = new Scanner(System.in);

        while (true) {
            System.out.println("\n==== Library Management System ====");
            System.out.println("1. Add Book");
            System.out.println("2. List Books");
            System.out.println("3. Issue Book");
            System.out.println("4. Return Book");
            System.out.println("5. Exit");
            System.out.print("Choose an option: ");

            int choice;
            try {
                choice = Integer.parseInt(scanner.nextLine());
            } catch (NumberFormatException e) {
                System.out.println("Invalid input. Please enter a number.");
                continue;
            }

            try {
                switch (choice) {
                    case 1:
                        System.out.print("Title: ");
                        String title = scanner.nextLine();
                        System.out.print("Author: ");
                        String author = scanner.nextLine();
                        library.addBook(title, author);
                        break;

                    case 2:
                        library.listBooks();
                        break;

                    case 3:
                        System.out.print("Book ID: ");
                        int issueId = Integer.parseInt(scanner.nextLine());
                        System.out.print("Member Name: ");
                        String memberName = scanner.nextLine();
                        library.issueBook(issueId, memberName);
                        break;

                    case 4:
                        System.out.print("Book ID: ");
                        int returnId = Integer.parseInt(scanner.nextLine());
                        library.returnBook(returnId);
                        break;

                    case 5:
                        System.out.println("Goodbye!");
                        return;

                    default:
                        System.out.println("Invalid option.");
                }
            } catch (Exception e) {
                System.out.println("Error: " + e.getMessage());
            }
        }
    }
}
