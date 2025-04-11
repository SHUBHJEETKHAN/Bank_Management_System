
# Bank Account Management System

## Overview
This is a simple **Bank Account Management System** implemented in C, designed as a CGI (Common Gateway Interface) application to manage bank accounts via a web interface. It allows users to create accounts, view all accounts, deposit money, withdraw money, and remove accounts. The account data is stored in a text file (`bank_accounts.txt`), and a separate meta file (`account_meta.txt`) tracks the last assigned account number.

The program is intended for educational purposes, demonstrating file handling, struct usage, and basic web integration with C.

## Features
- **Create Account**: Add a new account with a name and initial balance.
- **View Accounts**: Display all accounts in an HTML table with name, account number, and balance.
- **Deposit Money**: Add funds to an existing account by account number.
- **Withdraw Money**: Withdraw funds from an account, with validation for sufficient balance.
- **Remove Account**: Delete an account by account number.
- **Persistent Storage**: Account data is saved in `bank_accounts.txt`, and account numbers are tracked in `account_meta.txt`.
- **Web Interface**: Outputs HTML responses for integration with a web server via CGI.

## Prerequisites
To compile and run this program, you need:
- A C compiler (e.g., `gcc`).
- A web server with CGI support (e.g., Apache).
- Basic knowledge of setting up CGI scripts on a web server.

## Installation
1. **Clone the Repository**:
   ```bash
   git clone https://github.com/<your-username>/bank-account-management.git
   cd bank-account-management
   ```

2. **Compile the Program**:
   ```bash
   gcc bank.c -o bank.cgi
   ```

3. **Set Up the Web Server**:
   - Move the compiled `bank.cgi` to your web server's CGI directory (e.g., `/var/www/cgi-bin/` for Apache).
   - Ensure the CGI directory has execute permissions:
     ```bash
     chmod +x bank.cgi
     ```
   - Create a directory for data files (`bank_accounts.txt` and `account_meta.txt`) in a location writable by the web server (e.g., `/var/www/data/`).
   - Update the `FILE_NAME` and `META_FILE` constants in `bank.c` to point to this directory if needed.

4. **Configure File Permissions**:
   - Ensure the web server has read/write permissions for `bank_accounts.txt` and `account_meta.txt`:
     ```bash
     chmod 666 /path/to/data/*.txt
     ```

5. **Start the Web Server**:
   - Ensure your web server is running (e.g., `sudo systemctl start apache2` for Apache).

## Usage
Access the program through a web browser by sending HTTP queries to the CGI script. The program expects a query string in the format:
```
http://<server-address>/cgi-bin/bank.cgi?action=<action>&name=<name>&accNum=<account-number>&amount=<amount>
```

### Supported Actions
- **Create Account**:
  ```
  action=create&name=John%20Doe&amount=100.50
  ```
  Creates a new account for "John Doe" with an initial balance of $100.50.

- **View Accounts**:
  ```
  action=view
  ```
  Displays all accounts in an HTML table.

- **Deposit Money**:
  ```
  action=deposit&accNum=1001&amount=50.00
  ```
  Deposits $50.00 into account number 1001.

- **Withdraw Money**:
  ```
  action=withdraw&accNum=1001&amount=30.00
  ```
  Withdraws $30.00 from account number 1001 (if sufficient funds are available).

- **Remove Account**:
  ```
  action=remove&accNum=1001
  ```
  Deletes account number 1001.

### Example
To create an account:
```
http://localhost/cgi-bin/bank.cgi?action=create&name=Alice%20Smith&amount=200.00
```
Response:
```html
<html><body>Account created! Number: 1001</body></html>
```

To view all accounts:
```
http://localhost/cgi-bin/bank.cgi?action=view
```
Response:
```html
<html><body><h2>Bank Accounts</h2>
<table border='1'><tr><th>Name</th><th>Account No</th><th>Balance</th></tr>
<tr><td>Alice Smith</td><td>1001</td><td>200.00</td></tr>
</table></body></html>
```

## File Structure
- `bank.c`: The main C source code for the CGI program.
- `bank_accounts.txt`: Stores account data (name, account number, balance).
- `account_meta.txt`: Tracks the last assigned account number.
- `bank.cgi` (generated): The compiled CGI executable.

## Limitations
- **Security**: No input validation for malicious inputs (e.g., SQL injection is not applicable, but buffer overflows are possible).
- **Concurrency**: Not thread-safe; simultaneous access may corrupt `bank_accounts.txt`.
- **Scalability**: Limited to `MAX_ACCOUNTS` (100) due to array size.
- **Error Handling**: Basic error messages; no detailed logging.
- **Web Interface**: Minimal HTML output; no CSS or JavaScript for styling.

## Future Improvements
- Add input sanitization to prevent buffer overflows.
- Implement file locking for concurrent access.
- Use a database (e.g., SQLite) instead of text files for better scalability.
- Enhance the web interface with proper HTML forms and styling.
- Add authentication to secure account operations.

## Contributing
Contributions are welcome! To contribute:
1. Fork the repository.
2. Create a feature branch (`git checkout -b feature-name`).
3. Commit your changes (`git commit -m "Add feature"`).
4. Push to the branch (`git push origin feature-name`).
5. Open a pull request.

Please ensure your code follows the existing style and includes comments for clarity.

## License
This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Contact
For questions or suggestions, open an issue or contact the repository owner.

---

### Notes
- Replace `<your-username>` in the clone command with your actual GitHub username.
- If you want to include a `LICENSE` file, you can generate one on GitHub when creating the repository (MIT is suggested above).
- The README assumes a basic setup with Apache, but you can modify it if you use another web server (e.g., Nginx).
- The "Future Improvements" section is included to show potential enhancements, but you can remove it if you prefer a shorter README.

Let me know if you need help uploading this to GitHub or want to tweak the README further!
