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
    priority INTEGER NOT NULL
    )
""")
conn.commit()

st.title("🗄️ SQL Βάση Δεδομένων")
st.header("➕ Προσθήκη νέας εργασίας")

with st.form("task_form", clear_on_submit=True):
    task_title = st.text_input("Όνομα εργασίας:")
    task_priority = st.selectbox("Προτεραιότητα (1=Υψηλή, 5=Χαμηλή):", [1, 2, 3, 4, 5])
    
    submit_button = st.form_submit_button("Αποθήκευση στην βάση")

if submit_button and task_title:
    cursor.execute(
        "INSERT INTO tasks (title, priority) VALUES (?, ?)", 
        (task_title, task_priority)
    )
    conn.commit()
    st.success(f"Το task '{task_title}' αποθηκεύτηκε επιτυχώς!")


df = pd.read_sql_query("SELECT * FROM tasks", conn)

if not df.empty :
  st.dataframe(df, use_container_width=True)

conn.close()



import urllib
print("IP :", urllib.request.urlopen('https://ipv4.icanhazip.com').read().decode('utf-8').strip())

!streamlit run app.py & ssh -o StrictHostKeyChecking=no -R 80:localhost:8501 serveo.net
