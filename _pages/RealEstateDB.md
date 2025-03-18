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
    - **Oldest Open Maintenance Requests**: Prioritize urgent repairs.
    - **Running Payment Totals**: Track tenant payment history.
    - **Rent Yield Calculation**: Evaluate ROI per property.
    - **Maintenance Request Rankings**: Allocate resources efficiently.
    - **Owner Portfolio Valuation**: Assess investor equity.
- **Example**:
    
    ```sql
    -- Portfolio Value Calculation
    SELECT
      o.owner_id,
      SUM(p.purchase_price * (po.ownership_percentage / 100)) AS portfolio_value
    FROM PropertyOwner po
    JOIN Owner o ON po.owner_id = o.owner_id
    JOIN Property p ON po.property_id = p.property_id
    GROUP BY o.owner_id;
    
    ```
    
---

### **Code & Tools**

- [📂 GitHub Repo](https://github.com/jflores31297/RealEstateDB.git){: .btn .btn--info}
- **Key Files**:
    - `real_estate_crud.py`: Python CLI code.
    - `PropertyManagementDB.sql`: Database schema and sample data.

---

### **Challenges & Solutions**

1. **Database Optimization**:
    - **Problem**: Slow query performance with large datasets.
    - **Solution**: Added indexes on `property_id`, `status`, and `payment_date`.
2. **User Input Validation**:
    - **Problem**: Handling incorrect date/phone formats.
    - **Solution**: Regex validation and custom error messages.
3. **Scalability**:
    - **Problem**: Expanding the menu for new features.
    - **Solution**: Modular design with a dictionary-driven menu system.

---

### **Future Enhancements**

- **Filtering/Sorting**: Let users filter results by date range or sort columns.
- **Web Interface**: Migrate to a Flask/Django dashboard for broader accessibility.
- **Automated Reports**: Generate PDF/Excel reports from query results.

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
