rom flask import Flask, render_template, request
import os
import base64

app = Flask(_name_)

@app.route('/')
def camera():
    return render_template('camera.html')

@app.route('/save-photo', methods=['POST'])
def save_photo():
    username = request.form['username']  # Kullanıcı adını al
    photo_data = request.form['photo']  # Fotoğraf verisini al (Base64 formatında)
    
    # Fotorafi kaydet
    photo_data = photo_data.split(",")[1]  # Base64 header'ı temizle
    photo_path = os.path.join(os.getcwd(), f'{username}_photo.png')  # Kullanıcı adına göre isimlendir
    with open(photo_path, "wb") as photo_file:
        photo_file.write(base64.b64decode(photo_data))
    
    return f"Fotoğraf {username} tarafından kaydedildi!"

if _name_ == '_main_':
    app.run(debug=True)
