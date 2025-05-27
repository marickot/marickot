    #adminka
    @extends('layouts.app')
  
    @section('content')
    @section('content')
    <h1>Новые заявления</h1>
    </div>
    <table>
      <tr>
      <th>ФИО</th>
    
        <th>НОМЕР АВТО</th>
        <th>ОПИСАНИЕ</th>
        <th>СТАТУС</th>
      </tr>
    <tr>
    @foreach($statements as $order)
    <td>{{$order->user->name}}</td>
    <td>{{$order->number_auto}}</td>
    <td>{{$order->description}}
    <td>
        <form action="/admin/{{$order['id']}}"  method='POST'>
            @csrf
                    <legend>Смените статус:</legend>
                    <div>
                      <input type="radio" id="подтверждено" name="status" value="подтверждено" checked />
                      <label for="подтверждено">подтверждено</label>
                    </div>
    
                    <div>
                      <input type="radio" id="отклонено" name="status" value="отклонено" />
                      <label for="отклонено">отклонено</label>
                    </div>
            <button class='chs' type="submit">сменить статус</button>
    
        </form>
    </td>
    </tr>
    
    @endforeach
    </table>
    <h1>Все заявления</h1>
    
    <table>
      
    <tr>
    <th>ФИО</th>
      <th>НОМЕР АВТО</th>
      <th>ОПИСАНИЕ</th>
      <th>СТАТУС</th>
    </tr>
    @foreach($all as $order)
    <tr>
    <td>{{$order->user->name}}</td>
    <td>{{$order->number_auto}}</td>
    <td>{{$order->description}}
    <td>{{$order->status}}
    </td>
    </tr>
    @endforeach
    </table>
    @endsection
    #web.php
    <?php
    
    use Illuminate\Support\Facades\Route;
    
    Route::middleware('auth')->get('/', function () {
        return view('create');
    });
    
    Auth::routes();
    
    
    Route::middleware('auth')->get('/create', [App\Http\Controllers\HomeController::class, 'create'])->name('create');
    
    Route::middleware('auth')->post('/create_statem',  [App\Http\Controllers\HomeController::class, 'create_statem'])->name('create_statem');
    Route::middleware('auth')->get('/statements',  [App\Http\Controllers\HomeController::class, 'statements'])->name('statements');
    Route::middleware('admin')->get('/admin', [App\Http\Controllers\AdminController::class, 'admin'])->name('admin');
    Route::middleware('admin')->post('/admin/{id}', [App\Http\Controllers\AdminController::class, 'changeStatus'])->name('change');
