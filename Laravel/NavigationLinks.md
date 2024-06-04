# Navigation Links in Laravel/React

## General Steps

1. There should already have the Controller, Resources/js/Pages/File/.jsx

2. web.php should have already all the routes i.e. dashboard, project, task, view

2. Fill in the controller, function index : return inertia(XXXX,[]). Ensure the prop from the controller is passed down to the .jsx page for later stage

3. Ensure that the Resources/js/Pages/File/.js already has the following from its sibling the dashboard
    -props for the AuthenticatedLayout
    -Head from @inertiajs/react


## Parent Child Relationship

                             Dashboard -> AuthenticatedLayout

ProjectController -> Project/Index.jsx -> AuthenticatedLayout