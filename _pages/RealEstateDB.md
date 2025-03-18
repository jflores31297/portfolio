---
layout: single
classes: wide
---

<img src="https://github.com/jflores31297/portfolio/blob/main/assets/RealEstateDB-Cover.png?raw=true" width="900">

# Real Estate Management CLI with Advanced Analytics
*Python + MySQL | Terminal-Based Property Management System*
## **Overview**

A command-line interface (CLI) tool for managing real estate portfolios, featuring **CRUD operations** and **advanced SQL analytics**. Built to streamline property management tasks like lease tracking, maintenance prioritization, and investment performance analysis.

**Key Highlights**:

- **Advanced SQL Queries**: Window functions, CTEs, and dynamic joins for actionable insights.
- **Modular Design**: Easily expandable menu system for future queries.
- **Pagination & UX**: User-friendly navigation for large datasets.

**Tech Stack**:

- **Backend**: Python, MySQL, `mysql-connector`
- **Libraries**: `tabulate` (for table formatting), `logging`
- **Tools**: GitHub, SQL Workbench

---

# **Features**

### Database Design:
![ERD](https://github.com/jflores31297/portfolio/blob/main/assets/ERD.png?raw=true)

### **Core CRUD Operations**
- [▶️ CRUD Operations Demo](https://www.loom.com/share/f82c02702b344e7ca52bc4798e7421ff?sid=48414b6f-3f69-4d19-a24c-4ee4b39b280c){: .btn .btn--info}
- CRUD operations were implemented using MySQL queries executed through Python’s MySQL connector.
    - Create (C): New records (e.g., tenants, properties, leases, maintenance requests) are added using INSERT statements.
    - Read (R): Data retrieval is done via SELECT queries, including advanced analytics like ranking and aggregation. Pagination is integrated to handle large datasets efficiently.
	- Update (U): Records are modified using UPDATE statements, allowing changes to lease statuses, maintenance request progress, and tenant details.
	- Delete (D): DELETE statements are used to remove records, ensuring referential integrity by checking dependencies before deletion.
- The CLI provides a user-friendly interface for executing these operations, with error handling and confirmation prompts to prevent unintended modifications. 
- **Example:** Display (Read) All Owner Records

	```python
	# Function to view all owners with pagination
	def read_owners(cursor, page_size=10):
	    """
	    Retrieve and display all owners in a formatted table with pagination and error handling.
	
	    :param cursor: MySQL database cursor used to execute queries.
	    :param page_size: Number of records to display per page (default: 10).
	    """
	    try:
	        # Execute SQL query to fetch all owner records, ordered by owner_id
	        cursor.execute("SELECT * FROM Owner ORDER BY owner_id")
	        owners = cursor.fetchall()  # Fetch all records from the query result
	
	        # Check if the Owner table has any records
	        if not owners:
	            print("\nNo owners found.")
	            return  # Exit the function if no records exist
	
	        # Convert fetched records into a formatted list of dictionaries for better readability
	        formatted_owners = []
	        for owner in owners:
	            formatted_owner = {
	                "ID": owner[0],  # Owner ID
	                "First Name": owner[1],  # Owner's first name
	                "Last Name": owner[2],  # Owner's last name
	                "Email": owner[3],  # Email address
	                "Phone": owner[4] or "N/A",  # Handle NULL values by replacing with "N/A"
	                "Address": owner[5] or "N/A"  # Handle NULL values by replacing with "N/A"
	            }
	            formatted_owners.append(formatted_owner)
	
	        # Initialize pagination variables
	        total_owners = len(formatted_owners)  # Total number of owner records
	        start_index = 0  # Start index for displaying records
	
	        # Paginate through the results
	        while start_index < total_owners:
	            # Display a subset of owners as per the page size
	            print("\nOwner Records:")
	            print(tabulate(formatted_owners[start_index:start_index + page_size], headers="keys", tablefmt="grid", stralign="left"))
	            print(f"\nDisplaying {start_index + 1} to {min(start_index + page_size, total_owners)} of {total_owners} owners.")
	
	            # Check if there are more records to display
	            if start_index + page_size < total_owners:
	                # Prompt user to continue or quit pagination
	                next_page = input("\nPress Enter to view the next page or 'q' to quit: ").strip().lower()
	                if next_page == 'q':  # If user enters 'q', exit pagination loop
	                    break
	            start_index += page_size  # Move to the next page
	
	    except Error as e:  # Handle database-related errors
	        print(f"\nDatabase error: {e}")
	    except Exception as e:  # Handle any unexpected errors
	        print(f"\nAn unexpected error occurred: {e}")
	```
- **Output:**
<img src="https://github.com/jflores31297/portfolio/blob/main/assets/CRUD%20Output%20Screenshot.png?raw=true" width="900">
    

### **Advanced Analytics Submenu**
- [▶️ Advanced Analytics Demo](https://www.loom.com/share/f31faebb3b6749008fd29051bdcbaf61?sid=f1cf33ff-a85d-45d9-bb54-6b7bdd71d125){: .btn .btn--info}
- **5 Key Queries** (with easy expandability):
	1. Oldest Open Maintenance Requests: This query identifies the longest unresolved maintenance requests for each property, helping property managers prioritize urgent repairs and ensure tenant satisfaction.
	2. Running Payment Totals: Tracks cumulative tenant payments over time, allowing landlords to monitor rent payments, detect late payments, and analyze tenant payment behavior.
	3. Rent Yield Calculation: Computes the annual rental yield for each property, a key metric for assessing return on investment (ROI) and guiding investment decisions.
	4. Maintenance Request Rankings: Ranks properties based on the number of open maintenance requests, helping managers allocate resources efficiently and focus on properties with the most urgent repair needs.
	5. Owner Portfolio Valuation: Calculates the total property investment value for each owner based on ownership shares, allowing investors to assess their real estate holdings and make informed financial decisions.
	- Each query provides actionable insights for property managers, investors, and landlords, improving decision-making and operational efficiency.
- **Example 1:** Rank Properties by Open Maintenance Requests
	```python
	def rank_properties_by_open_requests(conn, page_size=5):
	    """
	    Retrieves and displays properties ranked by the number of open maintenance requests with pagination.
	
	    :param conn: Database connection object.
	    :param page_size: Number of records per page (default is 5).
	    """
	    query = """
	    WITH RankedRequests AS (
	        SELECT
	            p.address,
	            COUNT(mr.request_id) AS open_requests,
	            RANK() OVER (ORDER BY COUNT(mr.request_id) DESC) AS request_rank
	        FROM MaintenanceRequest mr
	        JOIN Property p ON mr.property_id = p.property_id
	        WHERE mr.status = 'Open'
	        GROUP BY p.property_id, p.address
	    )
	    SELECT * FROM RankedRequests LIMIT %s OFFSET %s;
	    """
	
	    try:
	        cursor = conn.cursor()
	        page = 0  # Start at page 0
	
	        while True:
	            offset = page * page_size
	            cursor.execute(query, (page_size, offset))
	            results = cursor.fetchall()
	
	            if not results and page == 0:
	                print("\n✅ No open maintenance requests found.")
	                break
	
	            if not results:
	                print("\n⚠️ No more records available.")
	                page -= 1  # Prevents moving beyond available pages
	                continue
	
	            print(f"\n🏠 Properties Ranked by Open Maintenance Requests (Page {page + 1}):")
	            print("Rank | Address                        | Open Requests")
	            print("-" * 70)
	
	            for row in results:
	                print(f"{row[2]:<4} | {row[0]:<30} | {row[1]:>5}")
	
	            # Pagination options
	            print("\n📖 Navigation: [N] Next Page | [P] Previous Page | [Q] Quit")
	            choice = input("Select an option: ").strip().lower()
	
	            if choice == "n":
	                page += 1
	            elif choice == "p" and page > 0:
	                page -= 1
	            elif choice == "q":
	                print("Returning to Advanced Queries menu...")
	                break
	            else:
	                print("❌ Invalid option. Please try again.")
	
	    except mysql.connector.Error as e:
	        print(f"❌ Database error: {e}")
	
	    finally:
	        cursor.close()
	```
 - **Output:**
<img src="https://github.com/jflores31297/portfolio/blob/main/assets/RankByMaintenanceReq.png?raw=true" width="900">

---

- **Example 2:** Calculate Each Owner's Portfolio Value
	```python
	def calculate_owner_portfolio_value(conn, page_size=5):
	    """
	    Retrieves and displays each owner's total portfolio value with pagination.
	
	    :param conn: Database connection object.
	    :param page_size: Number of records per page (default is 5).
	    """
	    query = """
	    SELECT
	        o.owner_id,
	        CONCAT(o.first_name, ' ', o.last_name) AS owner_name,
	        SUM(p.purchase_price * (po.ownership_percentage / 100)) AS portfolio_value
	    FROM PropertyOwner po
	    JOIN Owner o ON po.owner_id = o.owner_id
	    JOIN Property p ON po.property_id = p.property_id
	    GROUP BY o.owner_id
	    ORDER BY portfolio_value DESC
	    LIMIT %s OFFSET %s;
	    """
	
	    try:
	        cursor = conn.cursor()
	        page = 0  # Start at page 0
	
	        while True:
	            offset = page * page_size
	            cursor.execute(query, (page_size, offset))
	            results = cursor.fetchall()
	
	            if not results and page == 0:
	                print("\n✅ No owner portfolio data available.")
	                break
	
	            if not results:
	                print("\n⚠️ No more records available.")
	                page -= 1  # Prevents moving beyond available pages
	                continue
	
	            print(f"\n🏠 Owner Portfolio Values (Page {page + 1}):")
	            print("Owner ID | Owner Name          | Portfolio Value ($)")
	            print("-" * 60)
	
	            for row in results:
	                print(f"{row[0]:<8} | {row[1]:<20} | ${row[2]:,.2f}")
	
	            # Pagination options
	            print("\n📖 Navigation: [N] Next Page | [P] Previous Page | [Q] Quit")
	            choice = input("Select an option: ").strip().lower()
	
	            if choice == "n":
	                page += 1
	            elif choice == "p" and page > 0:
	                page -= 1
	            elif choice == "q":
	                print("Returning to Advanced Queries menu...")
	                break
	            else:
	                print("❌ Invalid option. Please try again.")
	
	    except mysql.connector.Error as e:
	        print(f"❌ Database error: {e}")
	
	    finally:
	        cursor.close()
	
	```
- **Output:**
<img src="https://github.com/jflores31297/portfolio/blob/main/assets/OwnerPortofolioValue.png?raw=true" width="900">
         
---

### **Code & Tools**

- [📂 GitHub Repo](https://github.com/jflores31297/RealEstateDB.git){: .btn .btn--info}
- **Key Files**:
    - `real_estate_crud.py`: Python CLI code.
    - `PropertyManagementDB.sql`: Database schema and sample data.

---

### **Challenges & Solutions**

1. **Database Optimization:**
	- Problem: As the dataset grew, queries became slower, especially when retrieving maintenance requests, payments, or rent yield calculations.
	- Solution: Indexes were added on property_id, status, and payment_date, significantly improving query execution times by reducing the number of rows scanned.
2. **User Input Validation:**
	- Problem: Users occasionally entered incorrect date formats (e.g., MM/DD/YYYY instead of YYYY-MM-DD) or invalid phone numbers.
	- Solution: Implemented regular expressions (regex) to enforce correct formats, along with custom error messages to guide users in entering valid data. This prevented database errors and improved data integrity.
3. **Scalability:**
	- Problem: As new features were added, the CLI menu became harder to manage and needed a more flexible way to expand.
	- Solution: A modular, dictionary-driven menu system was introduced, allowing new features to be added without modifying core logic. This made the system more maintainable and scalable.

---

### **Future Enhancements**
- **Filtering/Sorting:** Implementing filtering options (e.g., by date range, status, or tenant) and sorting functionalities (e.g., by rent yield, payment amount, or maintenance priority) would improve data usability and allow users to find relevant information faster.
- **Web Interface:** Migrating the CLI to a Flask or Django-based web dashboard would make the application more accessible, allowing users to interact with the system via a user-friendly interface rather than command-line inputs.
- **Automated Reports:** Adding functionality to export query results as PDF or Excel reports would enable property managers and investors to generate, share, and store structured financial and maintenance reports for better record-keeping and decision-making.

- These enhancements would significantly improve the application’s usability, accessibility, and efficiency for property management tasks.

---

### **Installation Guide**

```bash
# Clone the repo
git clone <https://github.com/yourusername/real-estate-cli.git>

# Install dependencies
pip install mysql-connector-python tabulate

# Set up MySQL database
mysql -u root -p < PropertyManagementDB.sql

# Run the CLI
python real_estate_crud.py

```
