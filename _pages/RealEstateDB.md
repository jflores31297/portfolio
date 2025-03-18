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
- **Example**:
    
	```python
	# Function to read and display all properties from the database
	def read_properties(cursor):
	    """
	    Retrieve and display all properties from the database in a formatted table with error handling.
	
	    :param cursor: MySQL database cursor used to execute queries.
	    """
	    try:
		# Execute SQL query to select relevant property details, ordered by property_id for consistency
		cursor.execute("""
		    SELECT 
			property_id, address, city, state, zip_code,
			property_type, square_feet, year_built,
			purchase_date, purchase_price
		    FROM Property
		    ORDER BY property_id
		""")
	
		# Fetch all records from the query result
		properties = cursor.fetchall()
	
		# Check if the database returned any properties
		if not properties:
		    print("\nNo properties found in the database.")  # Inform the user if no records exist
		    return  # Exit the function if no properties are found
	
		# Initialize an empty list to store formatted property data
		formatted_properties = []
	
		# Loop through each property record and format the data for better readability
		for prop in properties:
		    formatted_prop = {
			"ID": prop[0],  # Property ID
			"Address": prop[1],  # Street address
			"City": prop[2],  # City name
			"State": prop[3],  # State abbreviation
			"ZIP": prop[4],  # ZIP code
			"Type": prop[5],  # Property type (e.g., Single Family, Condo)
			"Sq Ft": f"{prop[6]:,}" if prop[6] else "N/A",  # Format square feet with commas, or show "N/A" if None
			"Year Built": prop[7] or "N/A",  # Display year built, or "N/A" if not available
			"Purchase Date": prop[8].strftime("%Y-%m-%d") if prop[8] else "N/A",  # Format date or show "N/A"
			"Price": f"${prop[9]:,.2f}" if prop[9] else "N/A"  # Format purchase price with thousands separator
		    }
		    formatted_properties.append(formatted_prop)  # Add formatted dictionary to the list
	
		# Print property listings in a tabular format using the tabulate library
		print("\nProperty Listings")  # Section header
		print(tabulate(formatted_properties, headers="keys", tablefmt="grid", stralign="left"))  # Generate a grid table
		print(f"\nTotal properties: {len(properties)}")  # Display total number of properties retrieved
	
	    # Handle MySQL database errors
	    except Error as e:
		print(f"\nDatabase error: {e}")  # Print database-related errors
	
	    # Handle any unexpected errors
	    except Exception as e:
		print(f"\nUnexpected error: {e}")  # Print general errors for debugging
	
	```

    

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
