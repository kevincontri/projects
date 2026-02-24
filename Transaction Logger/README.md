"""
*** CLI Transaction logger *** 
"""

"""
Transaction class represents each transaction object. With a given amount, category and transaction type. Each one also
has a unique (UUID) id and timestamp.
to_dict() method is used to create a dictionary out of every attribute of a given transaction object, in order to later
save it as a .json file
from_dict() is a class method that creates and returns a transaction object out of a given .json dict.
"""

"""
Ledger class represents an organized ledger for every transaction. It has a given name, so each ledger name will have it's own transactions,
for example, Personal ones, Business ones, etc.

At initialization a filename is already created (from the given ledger name); And a list is created to store every transaction from that
ledger; Also if that ledger name already exists in the current directory, it's loaded into memory with load_from_file() method.

add_transaction() is called within a specified ledger object, so users can add a specific transaction with the 'add' command on the CLI.
It receives the argument needed to create a Transaction object. Then that transaction object is added to the transactions list,
and it's automatically saved in a .json file.

_calculate_totals is where the calculations for total_income(), total_expense and balance is made, it loops through each transaction
in the transactions list and returns total income and expense. Each return value is used inside the mentioned methods for totals.

get_category_totals() receives a category as argument and checks totals (expense, income) only for a specified category.

report() returns a string with a summary of all transactions made in a specified ledger object along with total calculations.
Import to note that each transaction has a specific index in it, starting at 1, which makes easier for users to edit/delete a specific
transaction from a Ledger as will be shown.

save_to_file() creates a .json file containing all transactions from a Ledger, it uses list comprehension to generate 
a dictionary version of each dictionary, using to_dict()'s Transaction class method. Then it opens a .json file in writing 
mode, and uses json.dump() to write that list of dicts inside the file.

load_from_file() creates Transaction objects out of a .json file. It first empties the list of transaction in order to not
create duplicates of previous transactions from that Ledger, then opens the file in reading mode. The choice of using try/except
statements here was made due to the possibility of the file to be corrupted, therefore the exception 'json.JSONDecodeError' is 
stated, if it's corrupted, it starts with an empty list. If the file is not corrupted, each item (transaction) inside the .json
dict is created using the Transaction's class method from_dict(), explained before. After that the object is appended to the
transactions list.

delete_transaction() removes a specific transaction from the Ledger given it's index. It validates the index provided by the user,
then it pops the transaction out of the list and uses save_to_file() to make the change persistent and to happen in memory.

edit_transaction() alters an already provided attribute value, receiving an index by the user and the new attribute change. The index validation
is done in the cli() function. After the change is made, it returns True to indicate the edition was successful.
"""


"""
cli() does all the parsing of the Command Line Interface. All CRUD is done by it's commands and arguments, provided by the user.

The 'add' command creates a new transaction, and expects a ledger name (if non-existent a new one will be created, else will add
to an existing one), and an amount, a category and a transaction type. For validation, it checks if the amount provided is greater
than 0. 

The 'report' command reports all info (as stated above about the report() method) related to a ledger received as argument.

The 'report category' command returns total expenses and incomes related to a specified category. The method related does category
validation.

The 'delete' command deletes a transaction through an index passed as argument. The index validation is done by the delete_transaction
method. 

The 'edit' command edits an existing transaction through an index passed as argument. All args but the ledger name and index are optional arguments,
for a user can choose to edit one value or all values. If the edition was successful, a message will be printed out.
"""


