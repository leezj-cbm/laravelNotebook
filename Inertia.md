# Inertia

## In web.php

1. Traditionally return view() -> the app looks for pages in the Views for example BUT for return Intertia::render, the app looks for .jxs files in JS-> Pages


## App/Controller/Controller.js passing props to Resources/Pages/Edit.js

TLDR :- Take note of the props passed from the controller (parent) to the view page (child) !!!

Video https://www.youtube.com/watch?v=VrQRa-afCAk&list=LL , time 1:03:10 / 5:44:19

In the controller :

```js
    /**
     * Display the user's profile form.
     */
    public function edit(Request $request): Response
    {
        return Inertia::render('Profile/Edit', [
            'mustVerifyEmail' => $request->user() instanceof MustVerifyEmail,
            'status' => session('status'),
        ]);
    }


```


In the Resources/Pages/Edit.js :

mustVerifyEmail & status are passed from controller -> Edit.js file

```js
export default function Edit({ auth, mustVerifyEmail, status }) {
    return (
        <AuthenticatedLayout
            user={auth.user}
            header={<h2 className="font-semibold text-xl text-gray-800 dark:text-gray-200 leading-tight">Profile</h2>}
        >
            <Head title="Profile" />

            <div className="py-12">
                <div className="max-w-7xl mx-auto sm:px-6 lg:px-8 space-y-6">
                    <div className="p-4 sm:p-8 bg-white dark:bg-gray-800 shadow sm:rounded-lg">
                        <UpdateProfileInformationForm
                            mustVerifyEmail={mustVerifyEmail}
                            status={status}
                            className="max-w-xl"
                        />
                    </div>

                    <div className="p-4 sm:p-8 bg-white dark:bg-gray-800 shadow sm:rounded-lg">
                        <UpdatePasswordForm className="max-w-xl" />
                    </div>

                    <div className="p-4 sm:p-8 bg-white dark:bg-gray-800 shadow sm:rounded-lg">
                        <DeleteUserForm className="max-w-xl" />
                    </div>
                </div>
            </div>
        </AuthenticatedLayout>
    );
}


```

How about the Auth? Auth comes from the middleware !!!

Look for App/Http/Middleware/HandleInertiaRequests.php

Look at the bottom for this function

```js
 public function share(Request $request): array
    {
        return [
            ...parent::share($request),
            'auth' => [
                'user' => $request->user(),
            ],
        ];
    }

```

This functions in the middleware shares the auth to EVERY VIEW!



