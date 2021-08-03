## 'nondmon start'로 실행
![image](https://user-images.githubusercontent.com/30430227/127253400-28e54c2e-1f4a-455d-8ec5-01dd8e5adbbf.png)
> "main":"server.js" 실행파일명 지정

## express 에 body-parser 내장됨
```
    // app.use(bodyParser.urlencoded({extended:true}))
    app.use(express.urlencoded({extended:true}))
    app.use(express.json())
```
## mongoose 매서드 기능 확장 문제 - handlebars 에러
> 구버전을 사용하던가 Original JSON을 반환하는 '.lean()'옵션을 추가한다
```
router.get('/list',(req,res)=>{
    Employee.find((err,docs)=>{
        if(!err){
            res.render('employee/list',{
                list: docs
            })
        }
    }).lean() //lean 옵션 추가
})
```
## 서버, 라우터분리, db 모델
![image](https://user-images.githubusercontent.com/30430227/127324134-a99b4f9f-fd67-4099-85e1-e0ec74b545c3.png)
![image](https://user-images.githubusercontent.com/30430227/127324201-f6ebddc5-6a2a-44e5-97eb-b83f193aa940.png)

> server.js
```
const express = require('express')

const path = require('path')

// const bodyParser = require('body-parser')

const hbs = require('express-handlebars')

const app = express()

require('./models/db')

app.get('/',(req,res)=>{
    res.send('Hello world')
})

//bodyParser
    // app.use(bodyParser.urlencoded({extended:true}))
    app.use(express.urlencoded({extended:true}))
    app.use(express.json())
    
    //handlebars
    app.set('views', path.join(__dirname,'/views/'))
    
    app.engine('hbs',hbs({
        extname:'hbs',
        defaultLayout:'layout',
        layoutsdir:__dirname+'/views/layouts/'
    }))
    
    app.set('view engine', 'hbs')
    
    
    app.listen(3000,()=>{
        console.log('Server is listening Port 3000')
    })
    
    //별도 라우터 사용하기
    const employeeRouter =require('./routers/employeeRouter')
    app.use('/employee', employeeRouter) // '/employee' 경로
```

> 라우터 employeeController.js
```
const express = require('express')
const mongoose = require('mongoose')
const Employee = mongoose.model('Employee')


const router = express.Router()
// router.use(express.urlencoded({extended:true}))
// router.use(express.json())

router.get('/',(req,res)=>{
    res.render('employee/addOrEdit',{
        viewTitle:"Insert Employee"
    })
})

// 직원 입력
router.post('/',(req,res)=>{
    if(req.body._id==""){
        insertRecord(req, res)
    }else{
        updateRecord(req, res)
    }
})

// 직원 리스트 보기
router.get('/list',(req,res)=>{
    Employee.find((err,docs)=>{
        if(!err){
            res.render('employee/list',{
                list: docs
            })
        }
    }).lean()
})

function insertRecord(req,res)
{
    let employee = new Employee()
    employee.fullName = req.body.fullName
    employee.email = req.body.email
    employee.mobile = req.body.mobile

    employee.save((err, doc)=>{
        if(!err){
            res.redirect('employee/list')
        }else{
            if(err.name=='ValidationError'){
                handleValidationError(err, req.body)
                res.render('employee/addOrEdit',{
                    viewTitle:'Insert Employee',
                    employee:req.body
                })
            }
            console.log('Error occured during record insertion'+ err)
        }
    })
}

router.get('/:id',(req,res)=>{
    Employee.findById(req.params.id,(err,doc)=>{
        if(!err){
            res.render('employee/addOrEdit',{
                viewTitle:'Update Employee',
                employee:doc
            })
        }
    }).lean()
})

router.get('/delete/:id',(req,res)=>{
    Employee.findByIdAndRemove(req.params.id,(err,doc)=>{
        if(!err){
            res.redirect('/employee/list')
        }else{
            console.log("An error occured during the Delete Process"+ err)
        }
    }).lean()
})

function handleValidationError(err,body){
    for(field in err.errors)
    {
        switch(err.errors[field].path){
            case 'fullName':
                body['fullNameError']=err.errors[field].message
                break

            case 'email':
                body['emailError'] = err.errors[field].message
                break

                default:
                    break
        }
    }
}

function updateRecord(req,res)
{
// new:true 업데이트 후 문서를 반환한다(doc), runValidators: ValidationError 에러 체크(구문에러)
    Employee.findOneAndUpdate({_id:req.body._id}, req.body,{new:true, runValidators:true},(err,doc)=>{
        if(!err){
            res.redirect('employee/list')
        }else{
            if(err.name=="ValidationError"){
                handleValidationError(err, req.body)
                res.render("employee/addOrEdit",{
                    viewTitle:"Uddate Employee",
                    employee:req.body
                })
                console.log('Error occured in Updating the records'+err)
            }
    })
}

module.exports = router
```

> DB   db.js, employee.model.js
```
const mongoose = require('mongoose')

require('dotenv').config({path:'variables.env'})

// const url = "mongodb+srv://root:1234@education.8nfen.mongodb.net/myFirstDatabase?retryWrites=true&w=majority";

mongoose.connect(process.env.MONGODB_URL,{useNewUrlParser:true, useUnifiedTopology: true},(err) => {
    if(!err){ console.log("MongoDB Connection Succeeded");}
    else{
        console.log("An Error Occured");
    } 
})

require('./employee.model');
```
```
const mongoose = require('mongoose')
const validator = require('email-validator')

const employeeSchema = new mongoose.Schema({
    fullName:{
        type:String,
        required:'This field is required'
    },
    email:{
        type:String
    },
    mobile:{
        type:String
    }
})

// custom validation for email

employeeSchema.path('email').validate((val)=>{
    return validator.validate(val)
},'Invalid Email')


mongoose.model('Employee', employeeSchema)

```

> hbs  layout, addOrEdit, list
```
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style>
        *{
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            text-decoration: none;
        }
        html, body{
            height: 100%;
            background: rgb(211, 240, 255);
        }
        section{
            width: 60%;
            margin: 20px auto;
            padding: 50px 20px;
            background: white;
        }
        label{
            display: block;

        }
        form{
            margin: 10px 5px;
        }
        input[type='text']{
            width: 60%;
            border-radius: 5px;
            border: 1px solid gray;
            padding: 5px;
        }
        input[type='submit'], .viewAll{
            display: inline-block;
            margin: 10px 5px;
            background: firebrick;
            border: none;
            width: 100px;
            height: 30px;
            text-align: center;
            font-size: 12px;
            color: white;
            line-height: 30px;
            font-weight: bold;
            border-radius: 5px;
        }

        table{
            margin: 30px 10px;
            width: 100%;
            padding: 10px;
            background: white;
            border-top: 1px solid gray ;
            border-style: none;
            border-collapse: collapse;
        }
        th,td{
            border-bottom: 1px solid gray;
            padding: 10px 0;
        }
        tr:nth-child(even){
            background: rgb(238, 238, 238);
        }
        td{
            text-indent: 2em;
        }
        .createNew{
            display: inline-block;
            width: 130px;
            height: 40px;
            background: chocolate;
            color: white;
            line-height: 40px;
            border-radius: 5px;
            font-size: 14px;
        }
    </style>
</head>
<body>
    <section>
    {{{body}}}
    </section>
</body>
</html>
```
```
<h3>{{viewTitle}}</h3>

<form action="/employee" method="post" autocomplete="off">
    <input type="hidden" name="_id" value="{{employee._id}}">
    <div>
        <label for="">Full Name</label>
        <input type="text" name="fullName" value="{{employee.fullName}}" placeholder="Full Name">
        <div class="textDanger">
        {{employee.fullNameError}}
        </div>
    </div>
    <div>
        <label for="">Email</label>
        <input type="text" name="email" value="{{employee.email}}" placeholder="Email">
        <div class="textDanger">
        {{employee.emailError}}
        </div>
    </div>
    <div>
        <label for="">Mobile</label>
        <input type="text" name="mobile" value="{{employee.mobile}}" placeholder="Mobile">
    </div>
    <div>
        <input type="submit" value="🚀쿨쿨">
        {{!-- <span class="viewAll"><a href="#">View All</a></span> --}}
        <a href="/employee/list" class="viewAll">🛒View All</a>
    </div>
</form>
```
```
    <h2>
    <a href="/employee" class="createNew">➕Create New</a>
        Employee List</h2>
{{!-- {{list}} --}}
<table>
    <thead>
        <tr>
            <th>Full Name</th>
            <th>Email</th>
            <th>Mobile</th>
            <th></th>
        </tr>
    </thead>
    <tbody>
        {{#each list}}
        <tr>
            <td>{{this.fullName}}</td>
            <td>{{this.email}}</td>
            <td>{{this.mobile}}</td>
            <td><a href="/employee/{{this._id}}">🚀</a>
            <a href="/employee/delete/{{this._id}}" onclick="return confirm('정말로?')">♨</a>
            
            </td>
        </tr>
        {{/each}}

        
    </tbody>
</table>
```





