# Welcome to Code_Ib_Games

## To access the quiz, please create a username and password to log in - If you fail too many times in a row you will be directed to a login failure page

```
@app.route('/create_login', methods = ['GET', 'POST'])
def create_login():
    if request.method == 'POST':
        with open('data/user.txt', 'w') as f:
            username = request.form['userinput']
            f.write(username + "\n")
        with open(f'{username}pass.txt', 'w') as f:
            password = request.form['passinput']
            f.write(password + "\n")
    return render_template('create_login.html')
```

This piece of code allows you to create a Username and Password by storing it in txt file.

```
@app.route('/', methods=['GET', 'POST'])
def index():
    if request.method == 'POST': # To allow inputs from user to the page
        if request.form['username'] in open("data/user.txt", 'r').read() and request.form['password'] in open(f"{request.form['username']}pass.txt").read():
            # This reads to see if the username in the in the username file and if the password is correct. The password is stored in a unique file for each user
            return redirect(url_for('test'))
        elif len(open("data/attempts.txt", 'r').read()) > 2:
            file = open("data/attempts.txt", 'r+')
            file.truncate(0)
            file.close()
            return redirect(url_for('login_error'))
        elif request.form['username'] not in open("data/user.txt", 'r').read() or request.form['password'] not in open(f"{request.form['username']}pass.txt").read():
            with open('data/attempts.txt', 'a') as f:
                f.write("1")
    return render_template('index.html')
```
```@app.route('/home', methods=['GET', 'POST'])
def test():
    if request.method == 'POST':
        with open('templates/answers.html', 'w') as f:
            f.write("Test started at: " + str(datetime.now()))
        # First question
        score = 0
        if request.form['q1'] != "#":
            with open('templates/answers.html', 'a') as f:
                f.write("<br></br>" + "Q1: Wrong answer, the correct answer is: #")

        else:
            if request.form['q1'] == "#":
                with open('templates/answers.html', 'a') as f:
                    f.write("<br></br>" + "Q1: # is the correct answer")
                    score += 1

        if request.form['q2'] != "==":
            with open('templates/answers.html', 'a') as f:
                f.write("<br></br>" + "Q2: " + request.form['q2'] + " is the wrong answer, the correct answer is: ==")

        else:
            if request.form['q2'] == "==":
                with open('templates/answers.html', 'a') as f:
                    score += 1
                    f.write("<br></br>" + "Q2: == is the correct answer")
        with open('templates/answers.html', 'a') as f:
            f.write("<br></br>" + f"You have completed the test, your score is: {score}")
        return redirect(url_for('answers'))

    return render_template('testq.html')```

This creates a form that creates a list of question and stores the values in a html file that will create the answers page.

This is the login page code - By comparing the input values for username and password, it will determine whether you can make it to the next page
By tallying the number of failed attempts by appending a character to a text file each time the code fails, you can count the length of this file 
and once it has breached the number tries, it will redirect you to the login failed page