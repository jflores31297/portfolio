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

## **Features**

### Database Design:
![ERD](https://github.com/jflores31297/portfolio/blob/main/assets/ERD.png?raw=true)

### **Core CRUD Operations**
- [▶️ CRUD Operations Demo](https://www.loom.com/share/f82c02702b344e7ca52bc4798e7421ff?sid=48414b6f-3f69-4d19-a24c-4ee4b39b280c){: .btn .btn--info}
- Manage **properties, owners, tenants, leases, payments, and maintenance requests**.
- Input validation for dates, emails, phone numbers, and ownership percentages.
- **Example**:
    
    ```python
    def create_property(cursor):
        # Collects and validates property details (address, type, purchase price, etc.)
        # Inserts into MySQL with error handling
    
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
