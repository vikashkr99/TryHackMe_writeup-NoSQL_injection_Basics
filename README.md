<h1>TryHackMe - NoSQL injection Basics</h1>
<img src="./img/logo.png" alt="logo" width="400">
<h2>Task 1:</h2>
<ol type="1">
    <li>
        <h4>
            A group of documents in MongoDB is known as a... <br>
            <strong>Ans :</strong> <code> Collection </code>
        </h4>
    </li>
    <li>
        <h4>
            Using the MongoDB Operator Reference, find an operator to filter data when a field isn't equal to a given
            value<br>
            <strong>Ans :</strong> <code> $[ne] </code>
        </h4>
    </li>
    <li>
        <h4>
            Following the example of the 3 documents given before, how many documents would be returned by the following
            filter: ['gender' => ['$ne' => 'female'] , 'age' => ['$gt'=>'65'] ]<br>
            <strong>Ans :</strong> <code> 0 </code>
        </h4>
    </li>
</ol>
<h2>Task 3:</h2>
<ol type="1">
    <li>
        <h4>
            When bypassing the login screen using the <code>$ne</code> operator, which user are you logged in as?
            <ul>
                <li>Start machine attached in task 2.</li>
                <li>Visit the ip and you will see a login page.</li>
                <li>Turn on burpsuite to intercept the login request.</li>
                <li>Enter random values inside input fields. <strong>Dont use <code> admin </code> as username just
                        yet</strong>.</li>
                <li>Click on <code> login </code> and switch to burpsuite.</li>
                <li>
                    Add <code> [$ne] </code> after user and pass and send the request.<br>
                    <img src="./img/burp2.png" alt="burp2" width="500">
                </li>
                <li>Go back to browser.</li>
            </ul>
            <strong>Ans :</strong> <code> admin </code>
        </h4>
    </li>
</ol>
<h2>Task 4:</h2>
<strong>I will intercept the login request and send it to repeater so that i dont have to switch between firefox and
    burp all the time. Doing it with repeater will be easier</strong>
<ol type="1">
    <li>
        <h4>
            How many users are there in total?
            <ul>
                <li>Right click on the intercepted request in proxy and click on <code>send to repeater.</code> </li>
                <li>Add <code>[$nin][]</code> after <code>user</code> and <code>[$ne]</code> after <code>pass</code>
                </li>
                <li>Click on send.</li>
                <li>Click on <code> Follow Redirection </code></li>
                <li>You will see a new user name in the response tab now.</li>
                <li>This makes 2 users.</li>
                <li>Click on <code>&lt;</code> on the right of <code>cancel</code> button.</li>
                <li>Append <code>&user[$nin][]=pedro</code> to find the third user.</li>
                <li>Append this user on top of these two and we will find that we are getting redirected to the login
                    page.</li>
            </ul>
            <strong>Ans :</strong> <code> 3 </code>
        </h4>
        <strong>Watch this video for reference</strong><br>
        <a href="https://github.com/vikashkr99/TryHackMe_writeup-NoSQL_injection_Basics/blob/main/img/burpV.mov"> =>
            Video Link</a>
    </li>
    <li>
        <h4>
            There is a user that starts with the letter "p". What is his username?<br>
            <strong>Ans :</strong> <code>pedro</code>
        </h4>
    </li>
</ol>
<h2>Task 5:</h2>
<strong>
    You can read the task description and understand how we are going to brute force the password length first and then
    the password itself. Doing that manually can be painfull. So, to save my time i have written a pytohn script which will do the
    hard word for us.<br>
    <code>wget https://raw.githubusercontent.com/vikashkr99/pythonScripts/main/nosqlPasswordBrute.py</code><br>
    I have added comments to explain how it works in there.
</strong>
<ol type="1">
    <li>
        <h4>
            What is john's password?<br>
            Run the script like this.<br>
            <code> python3 nosqlPasswordBrute.py --host machineIp --user userName </code><br>
            and it will give you the password for that user.
            <img src="./img/script1.png" alt="script1"><br>
            <strong>Ans :</strong> <code> 10584312 </code>
        </h4>
    </li>
    <li>
        <h4>
            One of the users seems to be reusing his password for many services. Find which one and connect through SSH to retrieve the final flag!<br>
            <ul>
                <li>Bruteforce passwords using the script for the remaining two users.</li>
                <li>Try to login through ssh as <code>admin</code> <code>john</code> and <code>pedro</code> with their respective passwords.</li>
                <li>pedro and his password works for ssh.</li>
                <li><code>cat</code> the flag on desktop</li>
            </ul>
            <strong>Ans :</strong> <code>flag{N***********!}</code>
        </h4>
    </li>
</ol>