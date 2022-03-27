from flask import Flask,render_template,request,url_for,session,redirect,flash
from flask_mysqldb import MySQL

app=Flask(__name__)
app.secret_key="mysystem@12"


app.config['MYSQL_HOST']="remotemysql.com"
app.config['MYSQL_USER']="7uHI9MM9vC"
app.config['MYSQL_PASSWORD']="gUZhI4W7rX"
app.config['MYSQL_DB']="7uHI9MM9vC"
app.config['MYSQL_CURSORCLASS']="DictCursor"
mysql =MySQL(app)

@app.route('/',methods=['GET','POST'])
def index(): 
    if 'alogin' in request.form:
        if request.method=="POST":
            aname = request.form["aname"]
            apass = request.form["apass"]
            cur = mysql.connection.cursor()
            cur.execute("select * from Admin where aname=%s and apass=%s", [aname,apass])
            res = cur.fetchone()
            if res:
                session["aname"]=res["aname"]
                session["aid"]=res["aid"]
                return redirect(url_for('admin_home'))
            else:
                return render_template("index.html")
            mysql.connection.commit()
            cur.close()
    
    elif "register" in request.form:
        if request.method=='POST':
            uname = request.form["uname"]
            age = request.form["age"]
            dept = request.form["dept"]
            rno = request.form["rno"]
            upass = request.form["upass"]
            contact = request.form["contact"]
            cur = mysql.connection.cursor()
            cur.execute("insert into register (uname,age,dept,rno,upass,contact) values(%s,%s,%s,%s,%s,%s)",[uname,age,dept,rno,upass,contact])
            mysql.connection.commit()
        return render_template("index.html")
        
    elif "ulogin" in request.form:
        if request.method=="POST":
            uname = request.form["Uname"]
            upass = request.form["Upass"]
            cur = mysql.connection.cursor()
            cur.execute("select * from register where uname=%s and upass=%s", [uname,upass])
            res=cur.fetchone()
            if res:
                session["uname"]=res["uname"]
                session["uid"]=res["uid"]
                return redirect(url_for('user_home'))
            else:
                return render_template("index.html")
            mysql.connection.commit()
            cur.close()
    
    return render_template("index.html")
    
@app.route('/user_index',methods=['GET','POST'])
def user_index():
    if "booking" in request.form:
        if request.method=='POST':
            Name=request.form["name"]
            Register=request.form["register"]
            Card=request.form["card"]
            Type=request.form["type"]
            Genres=request.form["genres"]
            Book=request.form["book"]
            Date=request.form["date"]
            Return=request.form["return"]
            cur=mysql.connection.cursor()
            cur.execute("insert into library (NAME,RNO,CNO,TYPE,GENRES,BOOKNAME,DATE,RTNDATE) values(%s,%s,%s,%s,%s,%s,%s,%s)",[Name,Register,Card,Type,Genres,Book,Date,Return])
            mysql.connection.commit()
        return render_template("user_index.html")
    return render_template("user_index.html")
    
@app.route('/user_profile')
def user_profile():
    cur = mysql.connection.cursor()
    uid = session["uid"]
    qry = "select * from register where uid=%s"
    cur.execute(qry,[uid])
    data = cur.fetchone()
    cur.close()
    count = cur.rowcount
    if count == 0:
        flash("User Not Found...!!!","danger")
    return render_template("user_profile.html",res=data)
    
@app.route('/update_profile',methods=['GET','POST'])
def update_profile():
    if request.method=='POST':
        Name = request.form['Name']
        Age = request.form['Age']
        Dept = request.form['Dept']
        Rno = request.form['Rno']
        Password = request.form['Pass']
        Contact = request.form['Contact']
        uid = session["uid"]
        cur = mysql.connection.cursor()
        cur.execute("update register set uname=%s,age=%s,dept=%s,rno=%s,upass=%s,contact=%s where uid=%s", [Name,Age,Dept,Rno,Password,Contact,uid])
        mysql.connection.commit() 
        flash('User Updated...!!!','success')
        return redirect(url_for('user_profile'))
    return render_template("user_profile.html")

@app.route('/view_request')
def view_request():
    cur = mysql.connection.cursor()
    qry = "select * from library"
    cur.execute(qry)
    res = cur.fetchall()
    cur.close()
    count = cur.rowcount
    if count == 0:
        flash("No Requests Found...!!!","danger")
    return render_template("view_request.html",datas=res)

@app.route('/admin_home')
def admin_home():
    return render_template("admin_home.html")

@app.route('/user_home')
def user_home():
    return render_template("user_home.html")

@app.route('/logout')
def logout():
    session.clear()
    return redirect(url_for('index'))
    
if (__name__=='__main__'):
    app.run(debug=True)
