# Create Laravel Example Application
```
composer create-project laravel/laravel example-app
composer require laravel/ui

php artisan ui bootstrap
  
OR
  
php artisan ui bootstrap --auth

npm install && npm run dev

php artisan migrate
```

## Model, Migration, Factories and Seeders
```
php artisan make:model

  What should the model be named?
❯ Task

  Would you like any of the following? [none]
  none ........................................................................................................................................... 0  
  all ............................................................................................................................................ 1  
  factory ........................................................................................................................................ 2  
  form requests .................................................................................................................................. 3  
  migration ...................................................................................................................................... 4  
  policy ......................................................................................................................................... 5  
  resource controller ............................................................................................................................ 6  
  seed ........................................................................................................................................... 7
❯ 1
```
### TaskFactory.php
```
public function definition(): array
{
    return [
        'title' => fake()->sentence(),
        'description' => fake()->sentence(),
        'status' => rand(0, 3)
    ];
}
```
### DatabaseSeeder.php
```
Task::factory(10)->create();
```



```
php artisan make:resource TaskResource
```

### TaskResource.php
```
namespace App\Http\Resources;

use Illuminate\Http\Request;
use Illuminate\Http\Resources\Json\JsonResource;

class TaskResource extends JsonResource
{
    /**
     * Transform the resource collection into an array.
     *
     * @return array<int|string, mixed>
     */
    public function toArray(Request $request): array
    {
        return [
            'id' => $this->id,
            'title' => $this->title,
            'description' => $this->description,
            'status' => $this->status,
            'updated_at' => $this->updated_at,
            'created_at' => $this->created_at
        ];
    }
}
```

### TaskController
```
namespace App\Http\Controllers\Api\v1;

use App\Http\Controllers\Controller;
use App\Models\Task;
use App\Http\Requests\StoreTaskRequest;
use App\Http\Requests\UpdateTaskRequest;
use App\Http\Resources\TaskResource;

class TaskController extends Controller
{
    /**
     * Display a listing of the resource.
     */
    public function index()
    {
        return TaskResource::collection(Task::all());
    }

    /**
     * Store a newly created resource in storage.
     */
    public function store(StoreTaskRequest $request)
    {
        $item = Task::create($request->validated());
        return TaskResource::make($item);
    }

    /**
     * Display the specified resource.
     */
    public function show(Task $task)
    {
        return TaskResource::make($task);
    }

    /**
     * Update the specified resource in storage.
     */
    public function update(UpdateTaskRequest $request, Task $task)
    {
        $task->update($request->validated());
        return TaskResource::make($task);
    }

    /**
     * Remove the specified resource from storage.
     */
    public function destroy(Task $task)
    {
        $task->delete();
        return response()->noContent();
    }
}

```
### CompleteTaskController.php
```
namespace App\Http\Controllers\Api\v1;

use App\Http\Controllers\Controller;
use App\Http\Resources\TaskResource;
use App\Models\Task;
use Illuminate\Http\Request;

class CompleteTaskController extends Controller
{
    /**
     * Handle the incoming request.
     */
    public function __invoke(Request $request, Task $task)
    {
        $task->status = $request->status;
        $task->save();

        return TaskResource::make($task);
    }
}
```
### api.php
```
Route::prefix('v1')->group(function () {
    Route::apiResource('/tasks', TaskController::class);
    Route::patch('/tasks/{task}/status', CompleteTaskController::class);
});

```
## Adding Api Authentication



