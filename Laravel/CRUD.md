# CRUD

[Create](#create) , [Read](#read) , [Update](#update) , [Delete](#delete)

## Create

Note:

- Project is the model for the example below.

- These are the main files involved:-
  - JS/Pages/Project/Index -> link to Project/Create
  - JS/Pages/Project/Create -> useForm, forms, and submit
  - App/Http/Controller/ProjectController -> create function and store function
  - App/Http/Requests/StoreProjectRequest -> authorize and rules for validation
  - App/Models/ProjectModel -> fillable

1. Update the create function in ProjectController

```php
 public function create()
    {
        //
        return inertia("Project/Create");
    }
```

2. Establish form in Project.create.jsx , can use useForm form Inertia to do this. useForm is an object not an array. Can use html form and InputText, SelectText inertia components. Don't forget to put e.preventDefault() in the onSubmit function handler. Add in the onChange and onSubmit handler

```js
const { data, setData, post, errors, reset } = useForm({
  image: "",
  name: "",
  status: "",
  description: "",
  due_date: "",
});

const onChange = (e) => {
  setData({ ...data, [e.target.name]: e.target.value });
};

const onSubmit = (e) => {
  e.preventDefault();
  console.log("Project::create->onSubmit, data=" + Object.entries(data));
  post(route("project.store"));
};
```

3. In the ProjectController, go to store function. Notice that there is an input to the store function, which is the StoreProjectRequest.

4. Go to StoreProject Request under the Requests folder in App directory.

   - in the authorize function, return true
   - in the rules function, add all the requirement for validation

   ```php
     public function authorize(): bool
   {
       return true;
   }

   /**
    * Get the validation rules that apply to the request.
    *
    * @return array<string, \Illuminate\Contracts\Validation\ValidationRule|array<mixed>|string>
    */
   public function rules(): array
   {
       return [
           //
           "name"=>['required','max:255'],
           "description"=>['string'],
           "due_date"=> ['nullable','date'],
           "status"=>['required',Rule::in(['pending','in_progress','completed'])]
       ];
   }

   ```

5. Go to ProjectController and add in the store function as below.

```php
 public function store(StoreProjectRequest $request)
    {
        //
        $data = $request->validated();
        $data['created_by']= Auth::id();
        $data['updated_by']= Auth::id();
        Log::info('ProjectController:store=> '.json_encode($data));
        Project::create($data);
        return to_route('project.index');
    }
```

7. In the Project Model, update the fillable as below :-

```php
class Project extends Model
{
    use HasFactory;
    // ADD FILLABLE HERE!!!
    // Important note:- the fields here MUST match the database, else when saving the object, parameter will be nulL! Need to synchonize naming between model, migration and xxxResource (also StoreXXXRequest)!
    protected $fillable = ['img_path','name','description', 'status','due_date','created_by','updated_by'];

    public function tasks(){
        return $this->hasMany(Task::class);
    }

    public function createdBy(){
        return $this->belongsTo(User::class,'created_by');
    }

    public function updatedBy(){
        return $this->belongsTo(User::class,'updated_by');
    }
}


```

8. Test the submit. The post command in the onSubmit should send the data to the backend. Check the status using console.log, Log::info and network in BrowserInspect -> network.

9. Displaying of "success banner" in the page

   - Update the ProjectController store function

   ```php

   return to_route('project.index')->with('success','The project:'.(string)$request->name.' was created');

   ```

   - Update the ProjectController index function

   ```php
   return inertia("Project/Index", [
           "projects" => ProjectResource::collection($projects),
           'queryParams' => request()->query() ?: null,
           'routing' => 'project.index',
           'success'=> session('success'), // update this one!
       ]);

   ```

   - Update the Project/index to receive this information

   ```js
       //function to receive success
       export default function Index({ auth, projects, queryParams = null,routing ,success=null}) {
   ```

   ```js
   // display the banner
   {
     success && (
       <div className="bg-emerald-500 py-2 px-4 text-white rounded mb-4">
         {success}
       </div>
     );
   }
   ```

   10. Storing of images:-

   - In the Project/Create , change input to image

   ```js
   <div className="mt-4">
     <InputLabel htmlFor="project_image_path" value="Project Image" />
     <TextInput
       id="project_image_path"
       type="file"
       name="image"
       className="mt-1 block w-full"
       onChange={(e) => setData("image", e.target.files[0])}
     />
     <InputError message={errors.image} className="mt-2" />
   </div>
   ```

   - In the ProjectController, under store function, add code to receive image

   ```php
     public function store(StoreProjectRequest $request)
   {
       //
       $data = $request->validated();
       /** @var $image \Illuminate\Http\UploadedFile */ // --> add this in for Image
       $image=$data['image']?? null;                    //-->  add this in for Image
       $data['created_by']= Auth::id();
       $data['updated_by']= Auth::id();
       Log::info('ProjectController:store=> '.json_encode($data));
       if ($image){                                                         // --> add this in for Image
          $data['image']= $image->store('project/'.Str::random(),'public'); // --> add this in for Image
       }                                                                    //-->  add this in for Image
       Project::create($data);
       return to_route('project.index')->with('success','The project:'.(string)$request->name.' was created');
   }

   ```

   - create a storage link

   ```
       php artisan storage:link
   ```

   - The image file should now be stored in the Storage/App/Projectxyz/Public folder


## Read

- These are the main files involved:-
  - JS/Pages/Project/Index,Show,Edit -> pages which needs data and reads from DB via backend(ProjectController)

  - App/Http/Controller/ProjectController -> data handling in the backend where endpoints are exposed

  - App/Models/ProjectModel -> the fillable function is important for storing data. Fillable must match the DB side, else cannot write to DB!

  - App/Http/Resources/ProjectResource -> functions as the 'DTO' for communication FrontEnd(Project.Index)<>BackEnd(ProjectController). This is to serialize/deserialize from entity<>Json to display the data at front end

  - database/migrations/create_project_table -> the initial Entity/Object --> DB mapping

Take away points , in order to set up correct mapping:-

1. At the DB <>BackEnd , the following must 'key' must match. The 'key' must be exactly the same
    - database/migrations/create_project_table
    - App/Models/ProjectModel 
    - corresponding factory and seeders must have correct key and correct value type for initial population of db

2. At the BackEnd <>FrontEnd, in order to correctly display data:-
    - App/Http/Resources/ProjectResource - in the associative array -  
        - the key of associative array must match the ProjectController
        - the 'value' of associative array must match the Model and DB naming convention

    - App/Http/Controller/ProjectController - in the associative array:
        - the front end parameter names must match the 'key' of INERTIA associative array
        - the ProjectController converts entity to JSON using the ProjectResource
            - new ProjectResource($project) for single entity
            - ProjectResource::collection(%project) for multiple entities (a collection of entities)

## Update

- These are the main files involved:-
  - JS/Pages/Project/Index -> link to Project/Edit
  - JS/Pages/Project/Edit -> Page with the forms to update
  - App/Http/Controller/ProjectController -> edit function
  - App/Http/Requests/UpdateProjectRequest - > for validation of request?

1. Ensure Project.index edit link points to route(project.edit) and pass in the designated project to edit page

```js
    //inside the map function
    <Link
    href={route("project.edit", item)}
    className="font-medium text-blue-600 dark:text-blue-500 hover:underline mx-1"
    >
    Edit
    </Link>
```

2. Create an Project/Edit.jsx file. This file will be quite similar to the create file. There are some important differences compared to create!

```js
// function side
 const { data, setData, post, errors, reset } = useForm({
    image: "",
    name: project.name || "",
    status: project.status || "",
    description: project.description || "",
    due_date: project.dueDate || "",
    _method:'PUT', //---> this must be added in for INERTIA to work
  });

// return side
   const onSubmit = (e) => {
    e.preventDefault();
    console.log("Project::edit->onSubmit, data="+Object.entries(data));
    post(route("project.update",project.id)); // ----> although edit is a PUT method, we still use POST here for inertia to work , ALSO dont forget to pass in the project.id to the controller!
  };

```

3. In the ProjectController , we need to code for the edit method and the update method

```php
// edit method
 public function edit(Project $project)
    {
        //
        return inertia('Project/Edit',[
            'project'=> new ProjectResource($project),

        ]);
    }

```

```php
// update method

 public function update(UpdateProjectRequest $request, Project $project)
    {
        //
        $data = $request->validated();
        //dd($data); //-> data dump for testing purpose
        $image=$data['image']?? null;
        $data['updated_by']= Auth::id();
        if ($image){
            if($project->img_path){
                Storage::disk('public')->deleteDirectory(dirname($project->img_path));
            }
            $data['img_path']= $image->store('project/'.Str::random(),'public');
            Log::info("ProjectController:update=> Found image");
         }
         $project->update($data);
         Log::info('ProjectController:update=> '.json_encode($data));
         return to_route('project.index')->with('success','The project:'.(string)$request->name.' was updated');

    }

```

4. The update function accepts a parameter $request from UpdateProjectRequest, so we need to update two functions here

```php
 public function authorize(): bool
    {
        return true; // ---> set it to true from false
    }

public function rules(): array
    {
        return [
            //
            "name"=>['required','max:255'],  // ---- > all the required form validation are here
            "image"=>['nullable','image'],
            "description"=>['nullable','string'],
            "due_date"=> ['nullable','date'],
            "status"=>['required',Rule::in(['pending','in_progress','completed'])],
        ];
    }

```

5. Test out the edit function. It should work now!

## Delete

- These are the main files involved:-
  - JS/Pages/Project/Index -> link to Project/Create
  - App/Http/Controller/ProjectController -> destroy function

1. In the Project/Index page, ensure the delete link points to the ProjectController

```js
//Rendering Side - the item is under the map function
<button
  onClick={(e) => deleteProject(item)}
  className="font-medium text-red-600 dark:text-red-500 hover:underline mx-1"
>
  Delete
</button>
```

```js
//Function side - use router.delete , pass in the route and the project

const deleteProject = (theProject) => {
  if (!window.confirm("Are you sure you want to delete project?")) {
    return;
  }
  router.delete(route("project.destroy", theProject));
};
```

2. In the ProjectController, code the destroy() function to complete the backend. Send a succesfull message also.

```php
  public function destroy(Project $project)
    {
        //
        $project->delete();
        Log::info("ProjectController:destroy, deleted project ".Json_encode($project));
        return to_route('project.index')->with('success','Project '.(string)$project->name.' was deleted!');
    }

```
