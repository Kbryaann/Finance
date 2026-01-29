 CS50 Finance Project Summary

  This document outlines the implementation of the CS50 Finance web application,
  detailing the core functionalities, additional features, and the development
  process.

  Project Overview

  The goal was to implement C$50 Finance, a web application for managing stock
  portfolios. This tool allows users to register, log in, look up stock prices,
  buy and sell stocks, view their portfolio, and see a history of all
  transactions.

  Core Functionalities Implemented

  The following routes and their corresponding logic were implemented in app.py
  and supported by HTML templates:

   1. `/register` (User Registration)
       * Purpose: Allows new users to create an account.
       * Implementation Details:
           * Handles both GET (display form) and POST (process submission) requests.
           * Requires a username, password, and password confirmation.
           * Validates that all fields are non-blank.
           * Ensures the password and confirmation match.
           * Checks if the username already exists in the users table.
           * Hashes the user's password using generate_password_hash before storing
             it in the users table.
           * Redirects to the login page upon successful registration.
       * Files Created/Modified:
           * app.py: Implemented the /register route logic.
           * templates/register.html: Created the HTML form for registration.

   2. `/login` and `/logout` (User Authentication)
       * Purpose: Allows existing users to log in and log out.
       * Implementation Details:
           * /login handles GET (display form) and POST (process submission)
             requests.
           * Validates username and password against stored hashes.
           * Establishes a user session upon successful login.
           * /logout clears the user session.
       * Files Created/Modified:
           * app.py: Existing /login and /logout routes were utilized.
           * templates/login.html: Created the HTML form for login.

   3. `/quote` (Stock Quote Lookup)
       * Purpose: Allows users to look up the current price of a stock.
       * Implementation Details:
           * Handles both GET (display form) and POST (process submission) requests.
           * Requires a stock symbol.
           * Uses the lookup helper function to fetch stock information from an
             external API (IEX Cloud).
           * Renders quoted.html to display the stock's name, symbol, and price.
           * Renders an apology for invalid or blank symbols.
       * Files Created/Modified:
           * app.py: Implemented the /quote route logic.
           * templates/quote.html: Created the HTML form for entering a stock
             symbol.
           * templates/quoted.html: Created the HTML template to display the quoted
             stock price.

   4. `/buy` (Buy Stocks)
       * Purpose: Allows logged-in users to purchase shares of a stock.
       * Implementation Details:
           * Handles both GET (display form) and POST (process submission) requests.
           * Requires a stock symbol and a positive integer number of shares.
           * Validates the symbol using lookup.
           * Checks if the user has sufficient cash to complete the purchase.
           * Updates the user's cash balance in the users table.
           * Records the transaction (symbol, shares, price, timestamp) in the
             transactions table.
           * Redirects to the home page upon successful purchase.
       * Files Created/Modified:
           * app.py: Implemented the /buy route logic.
           * templates/buy.html: Created the HTML form for buying stocks.
           * finance.db: Modified to include a transactions table with user_id,
             symbol, shares, price, and timestamp columns, along with indexes on
             user_id and symbol.

   5. `/` (Index/Portfolio Display)
       * Purpose: Displays the user's current stock portfolio, cash balance, and
         total asset value.
       * Implementation Details:
           * Retrieves all stocks owned by the logged-in user from the transactions
             table, summing up shares for each symbol.
           * Fetches current prices for each owned stock using the lookup helper
             function.
           * Calculates the total value of each holding and a grand total (cash +
             total stock value).
           * Renders index.html to display this information in a formatted table.
       * Files Created/Modified:
           * app.py: Implemented the / route logic.
           * templates/index.html: Created the HTML table to display the portfolio
             summary.

   6. `/sell` (Sell Stocks)
       * Purpose: Allows logged-in users to sell shares of a stock they own.
       * Implementation Details:
           * Handles both GET (display form with a dropdown of owned stocks) and
             POST (process submission) requests.
           * Requires a selected stock symbol and a positive integer number of
             shares.
           * Validates that the user owns the specified number of shares.
           * Updates the user's cash balance in the users table.
           * Records the transaction (symbol, negative shares for selling, price,
             timestamp) in the transactions table.
           * Redirects to the home page upon successful sale.
       * Files Created/Modified:
           * app.py: Implemented the /sell route logic.
           * templates/sell.html: Created the HTML form for selling stocks,
             including a dynamic dropdown of owned stocks.

   7. `/history` (Transaction History)
       * Purpose: Displays a complete history of all buy and sell transactions for
         the logged-in user.
       * Implementation Details:
           * Retrieves all transactions for the current user from the transactions
             table, ordered by timestamp.
           * Renders history.html to display each transaction's symbol, shares,
             price, and timestamp.
       * Files Created/Modified:
           * app.py: Implemented the /history route logic.
           * templates/history.html: Created the HTML table to display the
             transaction history.

  Additional Features (Personal Touch)

   1. `/change_password` (Change Password)
       * Purpose: Allows logged-in users to change their account password.
       * Implementation Details:
           * Handles both GET (display form) and POST (process submission) requests.
           * Requires the user's current password, a new password, and new password
             confirmation.
           * Validates that all fields are non-blank and that the new password and
             confirmation match.
           * Verifies the current password against the stored hash.
           * Updates the user's password in the users table with the hash of the new
              password.
           * Redirects to the home page with a success message upon successful
             password change.
       * Files Created/Modified:
           * app.py: Implemented the /change_password route logic.
           * templates/change_password.html: Created the HTML form for changing the
             password.
           * templates/layout.html: Added a link to the /change_password route in
             the navigation bar.

  File Structure

  The project files are organized as follows within the
  p2p-lending-app/backend/src/finance directory:

    1 finance/
    2 ├── app.py
    3 ├── finance.db
    4 ├── helpers.py
    5 ├── requirements.txt
    6 ├── static/
    7 │   └── styles.css
    8 └── templates/
    9     ├── apology.html
   10     ├── buy.html
   11     ├── change_password.html
   12     ├── history.html
   13     ├── index.html
   14     ├── layout.html
   15     ├── login.html
   16     ├── quote.html
   17     ├── quoted.html
   18     ├── register.html
   19     └── sell.html

  How to Run the Application

   1. Navigate to the project directory:
      cd p2p-lending-app/backend/src/finance
   2. Install the required packages:
      pip install -r requirements.txt
   3. Set the `API_KEY` environment variable: You will need to obtain an API key frm
       IEX Cloud (https://iexcloud.io/).
      For example: export API_KEY="YOUR_IEX_CLOUD_API_KEY"
   4. Run the Flask application:
      flask run
   5. Open your web browser and navigate to the URL provided by Flask (usually
      http://127.0.0.1:5000/).
