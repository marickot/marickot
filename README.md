    main.blade
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
        <link rel="stylesheet" href="{{asset('css/style.css')}}">
        <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Unbounded:wght@200..900&display=swap" rel="stylesheet">
    </head>
    <body>
        <header>
           <a href="/" style='border:none'> <p >МОЙ НЕ САМ</p></a>
            <div>
                @guest
                <a href="{{route('register')}}">зарегистрироваться</a>
                <a href="{{route('login')}}">войти</a>
                @endguest
                @auth
                <a href="/">мои заявки</a>
                @if(Auth::user()->role == 'admin')
                <a href="/admin">панель администратора</a>
                @endif
                <form action="logout" method='POST'>
                @csrf
                <a ><button type="submit">выйти</button></a>
                </form>
                @endauth
            </div>
        </header>
        <div class="content">
        @yield('content')
    </div>
    </body>
    
    login.blade
    @extends('main')
    @section('content')
    <h1 class='name_form nn'>Вход</h1>
    <form method="POST" action="{{ route('login') }}" class='register'>
                            @csrf
                                <label for="login" class="col-md-4 col-form-label text-md-end">Логин</label>
                                    <input id="login" type="text" class="form-control @error('email') is-invalid @enderror" name="login" value="{{ old('email') }}" required autocomplete="email" autofocus>
                                    @error('login')
                                        <span class="invalid-feedback" role="alert"><strong>{{ $message }}</strong></span>
                                    @enderror
                                <label for="password" class="col-md-4 col-form-label text-md-end">Пароль</label>
                                    <input id="password" type="password" class="form-control @error('password') is-invalid @enderror" name="password" required autocomplete="current-password">
                                    @error('password') 
                                    <span class="invalid-feedback" role="alert"><strong>{{ $message }}</strong> </span>
                                    @enderror
                                    <button type="submit" class="btn btn-primary">войти</button>
                        </form>
    @endsection
    
    register.blade
    @extends('main')
    @section('content')
    <h1 class="name_form nn">Регистрация</h1>
    <form method="POST" action="{{ route('register') }}" class='register' >
                            @csrf
                                <label for="full_name" class="col-md-4 col-form-label text-md-end">ФИО</label>
                                    <input  type="text"  name="full_name" value="{{ old('full_name') }}" required autocomplete="full_name" autofocus>
                                    @error('name')
                                        <span class="invalid-feedback" role="alert"><strong>{{ $message }}</strong></span>
                                    @enderror
                                <label for="login" class="col-md-4 col-form-label text-md-end">Логин</label>
                                <input  type="text"  name="login" value="{{ old('login') }}" required autocomplete="login" autofocus>
                                    @error('login')
                                        <span class="invalid-feedback" role="alert"><strong>{{ $message }}</strong></span>
                                    @enderror
                                <label for="phone" class="col-md-4 col-form-label text-md-end">Номер телефона</label>
                                <input  type="phone"  name="phone" value="{{ old('phone') }}" required autocomplete="phone" autofocus>
                                    @error('phone')
                                        <span class="invalid-feedback" role="alert"><strong>{{ $message }}</strong></span>
                                    @enderror
                                <label for="email">Электронная почта</label>
                                    <input id="email" type="email" name="email" value="{{ old('email') }}" required autocomplete="email">
                                    @error('email')
                                        <span class="invalid-feedback" role="alert"><strong>{{ $message }}</strong></span>
                                    @enderror
                                <label for="password" class="col-md-4 col-form-label text-md-end">Пароль</label>
                                    <input id="password" type="password" name="password" required autocomplete="new-password">
                                    @error('password')
                                        <span class="invalid-feedback" role="alert">
                                            <strong>{{ $message }}</strong>
                                        </span>
                                    @enderror
                                <label for="password-confirm" class="col-md-4 col-form-label text-md-end">Повторите пароль</label>
                                    <input id="password-confirm" type="password" class="form-control" name="password_confirmation" required autocomplete="new-password">
                                    <button type="submit" class="btn btn-primary">зарегистрироваться</button>
                        </form>
    @endsection
    
    welcome.blade
    @extends('main')
    @section('content')
    <div class='cont'>
    <h1>Мои заявки</h1>
    <a href="{{route('new_task')}}">оставить заявку</a>
    </div>
    <div class='overflow'>
    <table>
      <tr>
        <th>ФИО</th>
        <th>АДРЕС</th>
        <th>ТЕЛЕФОН</th>
        <th>ДАТА</th>
        <th>ВРЕМЯ</th>
        <th>ТИП УБОРКИ</th>
        <th>ТИП ОПЛАТЫ</th>
        <th>СТАТУС</th>
      </tr>
    <tr>
    @foreach($orders as $order)
    <td>{{$order->name}}</td>
    <td>{{$order->address}}</td>
    <td>{{$order->phone}}</td>
    <td>{{$order->date}}</td>
    <td>{{$order->time}}</td>
    <td>{{$order->type}}</td>
    <td>{{$order->type_of_payment}}</td>
    <td>{{$order->status}}
    @if($order->status == 'отклонено')
    <br> {{$order->desc?->description}}
    @endif
    </td>
    </tr>
    @endforeach
    </div>
    @endsection
    
    new_task
    @extends('main')
    @section('content')
    <p class="name_form">Оставить заявку</p>
    <form action="{{route('create_task')}}" class="register" method='POST'>
    @csrf
    <label for="address">Адрес</label>
    <input type="text" name="address" id="" required>
    <label for="name">Имя</label>
    <input type="tel" name="name" id=""  value="{{Auth::user()->full_name}}" required>
    <label for="phone">Телефон</label>
    <input type="tel" name="phone" id="" value="{{Auth::user()->phone}}" required>
    <label for="date">Дата</label>
    <input type="date" name="date" id="" required>
    <label for="time">Время</label>
    <input type="time" name="time" id="" required>
    <label for="type">Тип уборки</label>
    <select name="type" id="">
        <option value="общий клининг">общий клининг</option>
        <option value="генеральная уборка">генеральная уборка</option>
        <option value="послестроительная уборка">послестроительная уборка</option>
        <option value="химчистка ковров и мебели">химчистка ковров и мебели</option>
    </select>
    <label for="type_of_payment">тип оплаты</label>
    <select name="type_of_payment" id="">
        <option value="наличные">наличные</option>
        <option value="карта">карта</option>
    </select>
    <button type="submit">оставить заявку</button>
    </form>
    @endsection
    
    admin
    @extends('main')
    @section('content')
    <h1>Здравствуйте, {{Auth::user()->full_name}}</h1>
    <div class="overflow">
    <table>
      <tr>
        <th>ФИО</th>
        <th>АДРЕС</th>
        <th>ТЕЛЕФОН</th>
        <th>ДАТА</th>
        <th>ВРЕМЯ</th>
        <th>ТИП УБОРКИ</th>
        <th>ТИП ОПЛАТЫ</th>
        <th>СТАТУС ЗАЯВКИ</th>
      </tr>
    <tr>
    @foreach($orders as $order)
    <td>{{$order->name}}</td>
    <td>{{$order->address}}</td>
    <td>{{$order->phone}}</td>
    <td>{{$order->date}}</td>
    <td>{{$order->time}}</td>
    <td>{{$order->type}}</td>
    <td>{{$order->type_of_payment}}</td>
    <td>
    @if($order->status == 'новое' or $order->status == 'в работе')
        <form action="/admin/{{$order['id']}}" method='POST'>
          @csrf
          <div style='display:flex; flex-direction:column;'>
              <label for="status">Текущий статус - {{$order->status}}</label>
                <fieldset>
                    <legend>Смените статус заявки:</legend>
                    <div>
                      <input type="radio" id="в работе" name="status" value="в работе" checked />
                      <label for="в работе">в работе</label>
                    </div>
                    <div>
                      <input type="radio" id="выполнено" name="status" value="выполнено" />
                      <label for="выполнено">выполнено</label>
                    </div>
                    <div class="checked">
                      <input type="radio" id="отклонено" name="status" value="отклонено" />
                      <label for="отклонено">отклонено</label>
                    <div class="toggle">
                        Укажите причину отказа:<br>
                        <input type="text" name="description" value="причина - ">
                    </div>
    </div>
    </fieldset>
    <button class='chs' type="submit">сменить статус</button>
          </div>
        </form>
    @endif
    @if($order->status == 'выполнено')
    <label for="status">Выполнено</label>
    @endif
    @if($order->status == 'отклонено')
    <label for="status">отклонено. {{$order->desc?->description}}</label>
    @endif
    </td>
    </tr>
    @endforeach
    </table>
    </div>
    @endsection
    
    style.css
    .unbounded-400 {
        font-family: "Unbounded", sans-serif; font-optical-sizing: auto; font-weight: 400; font-style: normal;font-variation-settings: "wdth" 100;
      }
    *{
        padding: 0; margin: 0; box-sizing: border-box; font-family: 'Unbounded'; color: #1e1e1e;
    }
    
    header{
        background-color: #ffffff; font-size: 22px;padding: 1vw 10vw;display: flex;flex-direction: row;align-items: center;
        justify-content: space-between;border-bottom: 1px solid black;
    }
    header p{
        font-size: 32px;
    }
    header div{
        display: flex; flex-direction: row;gap: 15px; 
    }
    header a {
        font-size: 22px; text-decoration: none;  display: flex;
        flex-direction: row; gap: 15px;border-radius: 10px; padding: 10px 15px; border: 1px solid black;
    }
    header button{
        font-size: 22px; text-decoration: none; border: none; background-color: unset;
    }
    .content{
        padding: 1vw 10vw;
    }
    .register{
        display: flex;flex-direction: column; width: 450px; padding: 25px;border-radius: 15px;
        border: 1px solid #1e1e1e;font-size: 18px;margin: auto;
    }
    .register input, .register select{
        padding: 8px 6px; margin-bottom: 5px;
    }
    .register label{
        margin-bottom: 5px;
    }
    
    .register button{
        color: #fff; background-color: #2e2e2e;  border: none; border-radius: 5px;
        font-size: 18px; padding: 15px 6px; margin-top: 5px;
    }
    
    .name_form{
        font-size: 28px;   text-align: center;  margin: 5px 0 15px 0 ;
    }
    
    table {
    	width: 100%;margin: 20px auto;border: 1px solid #dddddd;	border-collapse: collapse; 
    }
    table th {
        font-weight: bold;  padding: 5px; background: #efefef; font-size: 22px; border: 1px solid #dddddd; margin: 10px auto;
    }
    table td {
    	border: 1px solid #dddddd;padding: 5px;
    }
    .cont{
        display: flex; flex-direction: row; align-items: center;justify-content: space-between;margin-bottom: 10px;
    }
    .cont a{
        border: 1px solid #2e2e2e; text-decoration: none; color: #1e1e1e; border-radius: 15px; padding: 10px;font-size: 18px;
    }
    .checked .toggle {
        display:none;
    }
    
    .toggle input{
        padding: 10px; min-width: 300px;font-size: 18px;
    }
    fieldset{
        font-size: 18px;  margin: 10px 0;
    }
    .chs{
        color: #fff; font-size: 18px; background-color: #1e1e1e; border: none;border-radius: 5px;padding: 10px;
    }
    .checked input[type=radio]:checked ~ .toggle,
    .checked input[type=checkbox]:checked ~ .toggle
    {
        display:block;
    }
    .overflow{
        overflow-x: auto;
    }
    
    web.php
    <?php
    use Illuminate\Support\Facades\Route;
    use App\Http\Controllers\OrderController;
    use App\Http\Controllers\MainController;
    use App\Models\Order;
    use App\Models\Description;
    Route::middleware('auth')->get('/',[App\Http\Controllers\MainController::class, 'index']);
    Auth::routes();
    Route::get('/home', [App\Http\Controllers\HomeController::class, 'index'])->name('home');
    Route::middleware('auth')->get('/new_task', function () { return view('new_task');})->name('new_task');
    Route::middleware('auth')->post('/create_task', [App\Http\Controllers\OrderController::class, 'create'])->name('create_task');
    Route::middleware('auth')->get('/create_task',  function () { return view('new_task');});
    Route::middleware('admin')->get('admin', function(){  $orders = Order::all(); return view('admin',compact('orders'));})->name('admin');
    Route::middleware('admin')->post('/admin/{id}', [App\Http\Controllers\AdminController::class, 'changeStatus'])->name('change');
    
    admincontroller
    <?php
    namespace App\Http\Controllers;
    use Illuminate\Http\Request;
    use App\Models\Order;
    use App\Models\Description;
    class AdminController extends Controller {
        public function changeStatus(Request $request, $order){
            $order = Order::select()->where('id', $order)->first();
            $order->status = $request->status;
            $order->save();
            if ($request->status == 'отклонено'){
                $desc = new Description;
                $desc->order_id = $order->id;
                $desc->description = $request->description;
                $desc->save();
            }
            return redirect('/admin');
        }
    }
    
    main.controller
    <?php
    namespace App\Http\Controllers;
    use Illuminate\Http\Request;
    use App\Models\Order;
    use Illuminate\Support\Facades\Auth;
    class MainController extends Controller{
        public function index(){
            $orders = Order::where('user_id', Auth::user()->id)->get();
            return view('welcome', compact('orders'));
        }
    }
    
    ordercontroller
    <?php
    namespace App\Http\Controllers;
    use Illuminate\Http\Request;
    use App\Models\Order;
    use Illuminate\Support\Facades\Auth;
    class OrderController extends Controller{
        public function create(Request $values){
            $order = new Order;
            $order->create([
             'name'=> $values->name, 'phone' => $values->phone, 'user_id' => Auth::user()->id,
             'address' => $values->address, 'date' => $values->date,
              'time' => $values->time, 'type' => $values->type,
              'type_of_payment' => $values->type_of_payment
            ]);
           return redirect('/');
        }
    }
    
    user migration
            Schema::create('users', function (Blueprint $table) {
                $table->id();
                $table->string('full_name');
                $table->string('email')->unique();
                $table->string('phone')->unique();
                $table->string('login')->unique();
                $table->timestamp('email_verified_at')->nullable();
                $table->string('password');
                $table->enum('role', ['user', 'admin'])->default('user');
                $table->rememberToken();
                $table->timestamps();
            });
    
    orders migration
            Schema::create('orders', function (Blueprint $table) {
                $table->id();
                $table->unsignedBigInteger('user_id');
                $table->string('name');
                $table->string('phone');
                $table->string('address');
                $table->string('date');
                $table->string('time');
                $table->string('type');
                $table->string('type_of_payment');
                $table->enum('status', ['новое', 'в работе', 'выполнено', 'отклонено'])->default('новое');
                $table->timestamps();
                $table->foreign('user_id')->references('id')->on('users');
                    });
        }
    description migration
            Schema::create('descriptions', function (Blueprint $table) {
                $table->id();
                $table->unsignedBigInteger('order_id');
                $table->string('description');
                $table->timestamps();
                $table->foreign('order_id')->references('id')->on('orders');
            });
    order model
    class Order extends Model
    {
        protected $fillable = ['name', 'phone', 'address', 'user_id', 'date',  'time', 'type', 'type_of_payment'];
        public function desc(){
            return $this->hasOne(Description::class);
        }
    }
    
    description model
    use Illuminate\Database\Eloquent\Model;
    class Description extends Model{
        protected $fillable = ['description'];
    }
    
    middleware placeholder
        ->withMiddleware(function (Middleware $middleware) {
            $middleware->alias([
                'admin' => IsAdmin::class
            ]);
        })
