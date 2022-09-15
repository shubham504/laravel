# laravel
Basics of laravel 

# Routes:
     /routes/
```     
1) Route::get('/user/{id}', function ($id) {
         return 'User '.$id;
   });

2) Route::get('/user/{name}', function ($name) {
         return 'test';
   })->where('name', '[A-Za-z]+');

3) Route::get('/user/{id}/{name}', function ($id, $name) {
        return 'test';
   })->where(['id' => '[0-9]+', 'name' => '[a-z]+']);
```

# Controler path:
     /app/Http/Controllers/
```
use DB;
$users = DB::table('users')->get();
$users = DB::table('users')->where('email', '=', $request['email'])->get();
$users = DB::table('users')->where('email', '=', $request['email'])->get();
```
    
# Model path: 
    /app/
    
# View path:     
    /resources/views/
-----------------------------------------------------------------
# API
# Hiturl
     $siteURL="https://site url/";
     $siteURL/api/store/barcodefetch/
# Routes:
     /routes/
    ```
    use Illuminate\Http\Request;
    
    //Route::post('path url', 'controler name@controler function name');
    
    Route::post('/store/barcode', 'ApiController@storeBarcodeFetch');
    Route::get('/store/barcode', function()
     {
         return "Not allowed";
     });
    
    ```
 # Controler
     /app/Http/Controllers/
     
     ```
     namespace App\Http\Controllers;

     use Illuminate\Support\Facades\DB;
     use Illuminate\Http\Request;
     use App\Products;
     use App\Reports;
     use App\UserReports;

     class ApiController extends Controller
     {
         public function storeBarcodeFetch(Request $request) {
         
           $data = $request->all();
           $barcode=$data['barcode'];
           $results = DB::table('products')->where('barcode', '=', $barcode)->get();
           //print_r($results);
           return response()->json(array('status'=>1,'data'=>$results), 200);
         }
     }
     ```
   # Flash message
```
# controler:
     return redirect()->back()->with('message-success','Password changed!');
# blade template
     @if(session()->has('message-success'))
         <div class="alert alert-success">
             {{ session()->get('message-success') }}
         </div>
     @endif
```

  # Encript and decript
```
     $encryptVar = Crypt::encrypt($p_id); 
   
     $decryptVar = Crypt::decrypt($id);
```

-----------------------------------------------------------------------
-----------------------------------------------------------------------

# Laravel Sanctum 
What is Laravel Sanctum ?
Laravel Sanctum provides a featherweight authentication system for SPAs (single page applications), mobile applications, and simple, token based APIs. Sanctum allows each user of your application to generate multiple API tokens for their account. These tokens may be granted abilities / scopes which specify which actions the tokens are allowed to perform..

### You have to just follow a few steps to get following web services
##### Login API
##### Details API



------------------------------------------------------------------------
### Step 1: setup database in .env file

```` 
DB_DATABASE=youtube
DB_USERNAME=root
DB_PASSWORD= redhat@123
````

## Step 2:Install Laravel Sanctum.

````
composer require laravel/sanctum
````

## Step 3:Publish the Sanctum configuration and migration files .

````
php artisan vendor:publish --provider="Laravel\Sanctum\SanctumServiceProvider"

````

## Step 4:Run your database migrations.

````
php artisan migrate

````

## Step 5:Add the Sanctum's middleware.

````
../app/Http/Kernel.php

use Laravel\Sanctum\Http\Middleware\EnsureFrontendRequestsAreStateful;

...

    protected $middlewareGroups = [
        ...

        'api' => [
            EnsureFrontendRequestsAreStateful::class,
            'throttle:60,1',
            \Illuminate\Routing\Middleware\SubstituteBindings::class,
        ],
    ];

    ...
],

````

## Step 6:To use tokens for users.

````
use Laravel\Sanctum\HasApiTokens;

class User extends Authenticatable
{
    use HasApiTokens, Notifiable;
}

````

## Step 7:Let's create the seeder for the User model

```javascript 
php artisan make:seeder UsersTableSeeder
````

## Step 8:Now let's insert as record

```javascript 
use Illuminate\Support\Facades\DB;
use Illuminate\Support\Facades\Hash;
...
...
DB::table('users')->insert([
    'name' => 'John Doe',
    'email' => 'john@doe.com',
    'password' => Hash::make('password')
]);
````

## Step 9:To seed users table with user

```javascript 
php artisan db:seed --class=UsersTableSeeder
````


## Step 10:  create a controller nad  /login route in the routes/api.php file:


```javascript 
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use App\User;
use Illuminate\Support\Facades\Hash;
class UserController extends Controller
{
    // 

    function index(Request $request)
    {
        $user= User::where('email', $request->email)->first();
        // print_r($data);
            if (!$user || !Hash::check($request->password, $user->password)) {
                return response([
                    'message' => ['These credentials do not match our records.']
                ], 404);
            }
        
             $token = $user->createToken('my-app-token')->plainTextToken;
        
            $response = [
                'user' => $user,
                'token' => $token
            ];
        
             return response($response, 201);
    }
}


````


## Step 11: Test with postman, Result will be below

```javascript 

{
    "user": {
        "id": 1,
        "name": "John Doe",
        "email": "john@doe.com",
        "email_verified_at": null,
        "created_at": null,
        "updated_at": null
    },
    "token": "AbQzDgXa..."
}

````

## Step 11: Make Details API or any other with secure route  

```javascript 

Route::group(['middleware' => 'auth:sanctum'], function(){
    //All secure URL's

    });


Route::post("login",[UserController::class,'index']);

````
![Image of SSH.Ref1](https://github.com/shubham504/laravel/blob/c8224eaf80cdbfdc080047837ff7883a4a4cdb18/Screenshot%20from%202022-09-15%2021-04-02.png)
![Image of SSH.Ref2](https://github.com/shubham504/laravel/blob/c81360128526d360108aa5e171ab870bde6432b0/Screenshot%20from%202022-09-15%2021-07-59.png)

