1. Application Dependancies on operating system

- Install pip
- Install uwsgi and setup uwsgi app. refer to included inup.ini file
- Install nginx and setup upstream to use uwsgi app running on configured port
- Install and setup redis to run on 127.0.0.1
- Install mongodb from 10gen official repo. 

2. Install Dependancies
	Run:

	pip install -r requirement.txt 

3. Create mongodb database named - inup (case-sensitive)
4. Setup mongodb indexes by opening : https://your web address/setup
4. Setup admin user in mongodb. Run below command in mongodb console:
db.admins.insert({ "_id" : "admin", "password" : "pbkdf2:sha1:1000$pXXXYBZh$bc250f88a374c580eee61a7130f422efb30a9ac2", "full_name" : "Super Admin",   "last_login" : null,   "level" : 0})
5. Change below configurations in inup.py (on the root folder of INUP app)

```
INUP_CONNSTRING = "mongodb://127.0.0.1/inup" #change this to point to mongodb instance, if not running on 127.0.0.1

app = Flask(__name__, template_folder='templates')  
sys.path.append("/inup")
app.debug = True
app.config["INUP_CONNSTRING"] = INUP_CONNSTRING
app.config["UPLOAD_DIR"] = "/sites/inup/galpics/"  #change this path where you want to store uploaded files, also apply relevant permissions to the user running this flask app
app.wsgi_app = DebuggedApplication(app.wsgi_app, True)
app.secret_key = 'A0Zr98j/3yX R~XHH!jmN]LWX/,?RT'
app.session_interface = RedisSessionInterface(prefix="inup2session:") #Requires Redis instance config
app.register_blueprint(admin, url_prefix='/admin')
app.register_blueprint(index)

app.config.update(dict(
    DEBUG = True,
    MAIL_SERVER = '192.168.1.101', #change this to INUP SMTP server IP
    MAIL_PORT = 587, #change this to SMTP Server port
    MAIL_USE_TLS = True, #change this to "False" (case sensitive) if server does not support TLS
    MAIL_USE_SSL = False, #change this to "True" (case sensitive) if server support SSL
    MAIL_USERNAME = 'change_me', #change this to SMTP username
    MAIL_PASSWORD = 'change_password_here', #change this
))

```

