## 주소록
![image](https://user-images.githubusercontent.com/30430227/127465755-fb9bcd20-091d-47c2-8d79-8b982629eb35.png)

> server.js
```
const express = require('express')
const path = require('path')
const hbs = require('express-handlebars')
const { Server } = require('http')
const mongoose = require('mongoose')

const methodOverride = require('method-override')// HTML에 없는 put, delete 를 받는다


const app = express()

// DB
require('./models/db')

// Set
app.engine('hbs',hbs({
    extname:'hbs',
    defaultLayout:'layout',
    layoutsDir:__dirname+'/views/layouts/',
    partialsDir:__dirname+'/views/partials/'
}))
app.set('views', path.join(__dirname+'/views/'))

app.set('view engine','hbs')

// Middleware
app.use(express.static(__dirname+'/statics'))
app.use(express.urlencoded({extended: true})) //bodyParser
app.use(express.json())
app.use(methodOverride('_method')) // New

app.listen(3000,['192.168.0.33'],()=>{
    console.log('Server is running...')
})

// Router
const router = require('./controller/router')
app.use('/user', router)


```

> db.js
```
const mongoose = require('mongoose')

require('dotenv').config({path:'variables.env'})

mongoose.connect(process.env.MONGODB_URL, {useNewUrlParser:true, useUnifiedTopology:true},(err)=>{
    if(!err){console.log('MongoDB Connection succeeded')}
    else{
        console.log("An Error Occured")
    }
})

require('./user.model')

```

> user.model.js
```
const express = require('express')
const path = require('path')
const hbs = require('express-handlebars')
const { Server } = require('http')
const mongoose = require('mongoose')

const methodOverride = require('method-override')// HTML에 없는 put, delete 를 받는다


const app = express()

// DB
require('./models/db')

// Set
app.engine('hbs',hbs({
    extname:'hbs',
    defaultLayout:'layout',
    layoutsDir:__dirname+'/views/layouts/',
    partialsDir:__dirname+'/views/partials/'
}))
app.set('views', path.join(__dirname+'/views/'))

app.set('view engine','hbs')

// Middleware
app.use(express.static(__dirname+'/statics'))
app.use(express.urlencoded({extended: true})) //bodyParser
app.use(express.json())
app.use(methodOverride('_method')) // New

app.listen(3000,['192.168.0.33'],()=>{
    console.log('Server is running...')
})

// Router
const router = require('./controller/router')
app.use('/user', router)

```

> router.js
```
const express = require('express')
const mongoose = require('mongoose')


const User = mongoose.model('User')

const router = express.Router()

router.get('/',(req,res)=>{
    res.redirect('/user/contacts') // router redirect는 앞에 '/user/'가 붙는다
})

router.get('/contacts',(req,res)=>{
    User.find({},(err,docs)=>{ //  모든 데이터 받음, {}는 생략가능
        if(!err){
            res.render('contacts/index',{list:docs})
        }
    }).lean()
})

router.get('/contacts/new',(req,res)=>{
    res.render('contacts/new')
})

router.post('/contacts/new',(req,res)=>{
    User.create(req.body,(err,contact)=>{
        if(!err){res.redirect('/user')}
        else{
            return res.json(err)
        }
    })
    }
)

// Show
router.get('/contacts/:id',(req,res)=>{
    User.findOne({_id:req.params.id}, (err, doc)=>{ // findById(req.params.id 참고
        if(!err){
            res.render('contacts/show',{contact: doc})
        }else{
            return res.json(err)
        }
    }).lean()
})

// Edit
router.get('/contacts/:id/edit', (req, res)=>{
    User.findById(req.params.id, (err, doc)=>{
        if(!err){
            res.render('contacts/edit',{contact:doc})
        }
    }).lean()
})
router.put('/contacts/:id', (req,res)=>{
    User.findOneAndUpdate({_id:req.params.id},req.body,(err, doc)=>{
        if(!err){
            res.redirect('/user')
        }
    })
})

// Delete
router.delete('/contacts/:id',(req,res)=>{
    User.deleteOne({_id:req.params.id},(err)=>{ // findByIdAndRemove(req.params.id 참고
        if(err) return res.json(err)
        res.redirect('/user/contacts')
    })
})

module.exports = router
```

> layout.hbs
```
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <link rel="stylesheet" href="http://localhost:3000/style.css"> 
    {{!-- router 사용 시 절대경로 사용해야 오류가 없다 부모 경로가 루트, '/user' 현재 경로, --}}
</head>
<body>
    <nav>
        <div><h3>Contact Book</h3></div>
        <div><a href="/user/contacts">Index</a></div>
        <div><a href="/user/contacts/new">New</a></div>
    </nav>
    {{{body}}}
</body>
</html>
```

> new.hbs
```
<h2>New</h2>
<form action="/user/contacts/new" method="post">
    <div>
    <label for="name">Name</label>
    <input type="text" id="name" name="name">
    </div>
    <div>
    <label for="email">Email</label>
    <input type="text" id="email" name="email">
    </div>
    <div>
    <label for="phone">Phone</label>
    <input type="text" id="phone" name="phone">
    </div>

    <input type="submit" value="등록">
</form>
```

> show.hbs
```
<h2>Show</h2>
<ul>
    <li>
        이름은 : {{contact.name}}
    </li>
    <li>
        메일은 : {{contact.email}}
    </li>
    <li>
        전화 : {{contact.phone}}
    </li>
</ul>
<a href="/user/contacts/{{contact._id}}/edit">Edit</a>
<form action="/user/contacts/{{contact._id}}?_method=delete" method="post">
    <input type="submit" onclick="return confirm('정말?')" value="Delete">
</form>
```

> edit.hbs
```
<h2>Edit</h2>
<form action="/user/contacts/{{contact._id}}?_method=put" method="post">
    <div>
        <label for="name">Name</label>
        <input type="text" id="name" name="name" value="{{contact.name}}">
    </div>
    <div>
        <label for="email">Email</label>
        <input type="text" id="email" name="email" value="{{contact.email}}">
    </div>
    <div>
        <label for="phone">Phone</label>
        <input type="text" id="phone" name="phone" value="{{contact.phone}}">
    </div>
    <div>
        <input type="submit" value="쿨쿨">
    </div>
</form>
```

> style.css
```
*{
    margin: 0;
    padding: 0;
    text-decoration: none;
    list-style: none;
}

nav{
    color: lightgray;
    display: flex;
    flex-direction: row;
    width: 100%;
    height: 50px;
    align-items: center;
    background: ghostwhite;
}

nav div a{
    padding: 10px;
    color: gray;
    font-size: 12px;
    font-weight: bolder;
}
section{
    margin: 0 auto;
    width: 70%;
}
h2{
    color: tomato;
    margin-bottom: 20px;
}

ul{
    width: 100%;
    border: 1px solid lightgray;
    padding: 15px 10px;
}

label{
    display: inline-block;
    width: 80px;
    text-align: right;
}
input[type=submit]{
    border-style: none;
    width: 100px;
    margin-left: 100px;
}
```
