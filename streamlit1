!pip install streamlit -q
!npm install -g localtunnel -q




%%writefile app.py
import streamlit as st
import pandas as pd
import numpy as np

st.title("StreamLit App")
st.write("Αυτό το app τρέχει streamlit")

slider_val=st.slider("Διάλεξε έναν αριθμό", 1, 100, 50)
st.write=(f"Επέλεξεσ το {slider_val}")




import urllib
print("Password για το LocalTunnel : ", urllib.request.urlopen('https://ipv4.icanhazip.com').read().decode('utf-8').strip())
!streamlit run app.py & npx localtunnel --port 8501
