# CRUD

## C-Create

Note:- Project is the model for the example below

1. Make a Project.create.jsx
2. Update the create function in controller
3. Establish form in Project.create.jsx , can use useForm form Inertia to do this. useForm is an object not an array
4. In the ProjectController, go to store function. Notice that there is an input to the store function, which is the StoreProjectRequest. 
5. Go to StoreProject Request under the Requests folder in App directory.
    - in the authorize function, return true
    - in the rules function, add all the requirement

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