# Rails API

### Create New Rails App
rails new rails-api -d postgresql -T
cd rails-api
rails db:create

### Setup git:
git remote add origin https://github.com/learn-academy-2022-golf/wildlife-tracker-elysemontano.git
git branch -M main
git status
git add .
 git status
git commit -m "initial commit"
git push origin main
git checkout -b setup

### Add Rspec
bundle add rspec-rails
rails generate rspec:install


## Vocab
- API (Application Programming Interface) - transmits information across the internet as JSON

- JSON (Javascript Object Notation) - data structure that is supported by most programming languages

# Generate Resource
This will generate Migration file, model, controller, rspec files, views, helpers, routes (yes that means ALL our RESTful routes)
`rails g resource Student name:string cohort:string` 

Remove views (since we do not need them):
rm -r app/views/students

Informational command to see all of the routes available to us:
rails routes

Migrate to create schema file:
rails db:migrate

# RESTful Routes

Index: gets all instances from database
Show: gets one instance from database
Create: adds data to database for an instance
Update: updates existing instance in database
Destroy: removes instance from database

-- New: html form - do no need for API
-- Edit: html form: do no need for API


## Postman
GUI that allows us to display the data from an API

## Index

```ruby
  def index
    students = Student.all
    render json: students
  end
```

Postman: 
- Run server `$ rails s`
- URL: localhost:3000/students
- Make sure you have it set to JSON
- Send shows us all the instances as JSON!
- We can also see our progress in our terminal

## Show
```ruby
  def show
    student = Student.find(params[:id])
    render json: student
  end
```

Postman: 
- URL: localhost:3000/students/1
- Send shows us the instance as JSON

## Create
```ruby
  def create
    student = Student.create(student_params)
    if student.valid?
      render json: student
    else
      render json: student.errors
    end
  end

  private
  # strong params
  def student_params
    params.require(:student).permit(:name, :cohort)
  end
```

Postman:
- Change request to POST
- URL: localhost:3000/students
- Select: Body, Raw, Text -> JSON
- Setup params:
  ```json
  {
    "name": "Frank",
    "cohort": "Golf"
  }
  ```

To fix error Authenticity Token Error:

/app/controllers/application_controller.rb
```ruby
skip_before_action :verify_authenticity_token
```


## Update
```ruby
  def update
    student = Student.find(params[:id])
    student.update(student_params)
    if student.valid?
      render json: student
    else
      render json: student.errors
    end
  end
```

Postman:
  - Set URL: localhost:3000/students/2
  - Set to PATCH request
  - Update params
  - Send

## Destroy
```ruby
  def destroy
    student = Student.find(params[:id])
    if student.destroy
      render json: student
    else
      render json: student.errors
    end
  end
```

Postman: 
  - Change request to delete
  - Change URL: localhost:3000/students/2
  - We can see the student that was just removed show up
  - To double check that this request was successfully, let's do a get request to see all students
