# laravel
Basics of laravel 

# Routes:
     /routes/
    1) Route::get('/user/{id}', function ($id) {
         return 'User '.$id;
     });

    2) Route::get('/user/{name}', function ($name) {
         return 'test';
     })->where('name', '[A-Za-z]+');

    3) Route::get('/user/{id}/{name}', function ($id, $name) {
        return 'test';
     })->where(['id' => '[0-9]+', 'name' => '[a-z]+']); 
# Controler path:
     /app/Http/Controllers/
    
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
     
