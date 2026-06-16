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



st.header("Σορταρισμα")
sort_order= st.radio(
    "Ταξινόμηση",
    ["Κανονική", "Αύξουσα", "Φθίνουσα"],
    horizontal = True
)

if sort_order == "Αύξουσα":
  query = "SELECT * FROM tasks ORDER BY priority ASC"
elif sort_order == "Φθίνουσα":
  query = "SELECT * FROM tasks ORDER BY priority DESC"
else:
  query = "SELECT * FROM tasks"




df = pd.read_sql_query(query, conn)

if not df.empty :
  st.dataframe(df, use_container_width=True)
  col1, col2 = st.columns(2)

  with col1:
    st.header("Ενημέρωση εργασίας")
    available_ids_update = df['id'].tolist()
    id_to_update = st.selectbox("Επέλεξε id για αλλαγή", available_ids_update, key="update_id")
    new_title = st.text_input("Νέο όνομα εργασίας")

    if st.button("Ενημέρωση τίτλου"):
      cursor.execute("UPDATE tasks SET title = ? WHERE id = ?", (new_title, id_to_update))
      conn.commit()
      st.success(f"Το τασκ με id {id_to_update} ενημερώθηκε")
      st.rerun()


  with col2:
    st.header("Διαγραφή εργασίας")
    available_ids_delete = df['id'].tolist()
    id_to_delete = st.selectbox("Επέλεξε id για διαγραφή", available_ids_delete, key = "delete_id")

    if st.button("Διαγραφή εργασίας"):
      cursor.execute("DELETE FROM tasks WHERE id = ? ", (id_to_delete,))
      conn.commit()
      st.success(f"Το task με id {id_to_delete} διαγράφτηκε")
      st.rerun()




conn.close()






import urllib
print("IP :", urllib.request.urlopen('https://ipv4.icanhazip.com').read().decode('utf-8').strip())

!streamlit run app.py & ssh -o StrictHostKeyChecking=no -R 80:localhost:8501 serveo.net
