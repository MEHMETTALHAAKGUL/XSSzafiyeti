import sqlite3

def create_db():
    conn = sqlite3.connect("vulnerable.db")
    cursor = conn.cursor()
    cursor.execute("CREATE TABLE IF NOT EXISTS users (id INTEGER PRIMARY KEY, username TEXT, password TEXT)")
    cursor.execute("INSERT INTO users (username, password) VALUES ('admin', 'admin123')")
    conn.commit()
    conn.close()

def secure_login():
    conn = sqlite3.connect("vulnerable.db")
    cursor = conn.cursor()
    
    username = input("Kullanıcı adını gir: ")
    cursor.execute("SELECT * FROM users WHERE username = ?", (username,))  # Güvenli parametrik sorgu
    
    user = cursor.fetchone()
    if user:
        print("Giriş başarılı:", user)
    else:
        print("Kullanıcı bulunamadı.")
    
    conn.close()

if __name__ == "__main__":
    create_db()
    secure_login()

