import streamlit as st

# 1. Setup Web Page
st.set_page_config(page_title="UK Tax Engine", page_icon="🧮")
st.title("🧮 UK Tax & NIC Liability Engine")
st.write("An automated calculator processing salary, dividends, and band erosion.")

# 2. Web Interface Inputs (Replaces terminal input())
col1, col2 = st.columns(2)
with col1:
    salary = st.number_input("Enter Gross Salary (£)", min_value=0, value=45000, step=1000)
with col2:
    dividends = st.number_input("Enter Dividend Income (£)", min_value=0, value=2000, step=1000)

# 3. Calculation Logic (Runs when the user clicks the button)
if st.button("Calculate Tax Liability"):
    
    # Statutory Variables
    standard_pa = 12570
    basic_band_limit = 37700
    higher_band_limit = 125140
    div_allowance = 500
    nic_primary_threshold = 12570
    nic_upper_limit = 50270

    # NIC Calculation
    nic_total = 0
    if salary > nic_upper_limit:
        nic_total = ((nic_upper_limit - nic_primary_threshold) * 0.08) + ((salary - nic_upper_limit) * 0.02)
    elif salary > nic_primary_threshold:
        nic_total = (salary - nic_primary_threshold) * 0.08

    # Personal Allowance Taper
    total_income = salary + dividends
    if total_income > 100000:
        reduction = (total_income - 100000) / 2
        pa = max(0, standard_pa - reduction)
    else:
        pa = standard_pa

    # Allocate Income Stack
    taxable_salary = max(0, salary - pa)
    remaining_pa = max(0, pa - salary)
    taxable_dividends_gross = max(0, dividends - remaining_pa)
    taxable_dividends = max(0, taxable_dividends_gross - div_allowance)

    # Calculate Salary Tax
    salary_tax = 0
    basic_band_used = 0
    higher_band_used = 0

    if taxable_salary > 0:
        salary_basic = min(taxable_salary, basic_band_limit)
        salary_tax += salary_basic * 0.20
        basic_band_used = salary_basic
        
        if taxable_salary > basic_band_limit:
            salary_higher = min(taxable_salary - basic_band_limit, higher_band_limit - basic_band_limit)
            salary_tax += salary_higher * 0.40
            higher_band_used = salary_higher
            
        if taxable_salary > higher_band_limit:
            salary_tax += (taxable_salary - higher_band_limit) * 0.45

    # Calculate Dividend Tax
    dividend_tax = 0
    if taxable_dividends > 0:
        basic_band_remaining = max(0, basic_band_limit - basic_band_used)
        div_basic = min(taxable_dividends, basic_band_remaining)
        dividend_tax += div_basic * 0.0875  
        
        remaining_divs_after_basic = taxable_dividends - div_basic
        higher_band_capacity = (higher_band_limit - basic_band_limit) - higher_band_used
        div_higher = min(remaining_divs_after_basic, max(0, higher_band_capacity))
        dividend_tax += div_higher * 0.3375  
        
        div_additional = remaining_divs_after_basic - div_higher
        if div_additional > 0:
            dividend_tax += div_additional * 0.3935 

    # Totals
    total_tax = salary_tax + dividend_tax
    total_deductions = total_tax + nic_total
    take_home = total_income - total_deductions

    # 4. Web Interface Outputs (Replaces terminal print())
    st.divider()
    st.subheader("🧾 Liability Report")
    
    # Large metric displays
    res_col1, res_col2 = st.columns(2)
    res_col1.metric(label="Total Deductions (Tax + NICs)", value=f"£{total_deductions:,.2f}")
    res_col2.metric(label="Net Take-Home Pay", value=f"£{take_home:,.2f}")
    
    # Detailed breakdown
    st.write(f"**Gross Salary:** £{salary:,.2f} | **Gross Dividends:** £{dividends:,.2f}")
    st.write(f"**Personal Allowance Applied:** £{pa:,.2f}")
    
    st.caption("Tax Breakdown:")
    st.code(f"""
Class 1 NICs:           £{nic_total:,.2f}
Income Tax (Salary):    £{salary_tax:,.2f}
Income Tax (Dividends): £{dividend_tax:,.2f}
    """)
