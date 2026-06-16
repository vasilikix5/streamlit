!pip install streamlit -q
!npm install -g localtunnel -q





%%writefile app.py
import streamlit as st
import sqlite3
import pandas as pd

conn = sqlite3.connect('database.db', check_same_thread=False)
cursor = conn.cursor()

cursor.execute("""
CREATE TABLE IF NOT EXISTS tasks (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    title TEXT NOT NULL,
    priority INTEGER NOT NULL,
    date TEXT
)
""")
conn.commit()

st.title("🗄️ SQL Βάση Δεδομένων (Full CRUD & Search)")

st.header("➕ Προσθήκη νέας εργασίας")

with st.form("task_form", clear_on_submit=True):
    task_title = st.text_input("Όνομα εργασίας:")
    task_priority = st.selectbox("Προτεραιότητα (1=Υψηλή, 5=Χαμηλή):", [1, 2, 3, 4, 5])
    task_day = st.text_input("Ημέρα εβδομάδας:")
    
    submit_button = st.form_submit_button("Αποθήκευση στην βάση")

if submit_button and task_title:
    cursor.execute(
        "INSERT INTO tasks (title, priority, date) VALUES (?, ?, ?)", 
        (task_title, task_priority, task_day)
    )
    conn.commit()
    st.success(f"Το task '{task_title}' αποθηκεύτηκε επιτυχώς!")

st.header("📋 Live Δεδομένα από τον Πίνακα 'tasks'")

search_query = st.text_input("🔍 Αναζήτηση εργασίας με βάση τον τίτλο:")

sort_order = st.radio(
    "Ταξινόμηση ανά Προτεραιότητα:",
    ["Καμία (Φυσική σειρά)", "Αύξουσα (Υψηλή -> Χαμηλή)", "Φθίνουσα (Χαμηλή -> Υψηλή)"],
    horizontal=True
)

base_query = "SELECT * FROM tasks WHERE title LIKE ?"
params = (f"%{search_query}%",) 

if sort_order == "Αύξουσα (Υψηλή -> Χαμηλή)":
    base_query += " ORDER BY priority ASC"
elif sort_order == "Φθίνουσα (Χαμηλή -> Υψηλή)":
    base_query += " ORDER BY priority DESC"

df = pd.read_sql_query(base_query, conn, params=params)

if not df.empty:
    st.dataframe(df, use_container_width=True)
    
    col1, col2 = st.columns(2)
    
    with col1:
        st.header("🔄 Ενημέρωση Εργασίας")
        available_ids_update = df['id'].tolist()
        id_to_update = st.selectbox("Επίλεξε ID για αλλαγή:", available_ids_update, key="update_id")
        new_title = st.text_input("Νέο Όνομα Εργασίας:")
        
        if st.button("💾 Ενημέρωση Τίτλου"):
            if new_title:
                cursor.execute("UPDATE tasks SET title = ? WHERE id = ?", (new_title, id_to_update))
                conn.commit()
                st.success(f"Το task με ID {id_to_update} ενημερώθηκε!")
                st.rerun()
            else:
                st.warning("Παρακαλώ γράψε ένα νέο όνομα.")

    with col2:
        st.header("🗑️ Διαγραφή Εργασίας")
        available_ids_delete = df['id'].tolist()
        id_to_delete = st.selectbox("Επίλεξε ID για διαγραφή:", available_ids_delete, key="delete_id")
        
        if st.button("❌ Διαγραφή Επιλεγμένου Task"):
            cursor.execute("DELETE FROM tasks WHERE id = ?", (id_to_delete,))
            conn.commit()
            st.success(f"Το task με ID {id_to_delete} διαγράφηκε!")
            st.rerun()
        
else:
    st.info("Δεν βρέθηκαν εργασίες.")

conn.close()






import urllib
print("IP :", urllib.request.urlopen('https://ipv4.icanhazip.com').read().decode('utf-8').strip())

!streamlit run app.py & ssh -o StrictHostKeyChecking=no -R 80:localhost:8501 serveo.net
