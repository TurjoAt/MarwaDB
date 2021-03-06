## MarwaDB for MarwaPHP Framework

**MarwaDB** is php mysql library for **MarwaPHP** framework based on PDO. It is robust, faster and simple. No External Library has been used. It is raw and simple PHP Mysql Library by focusing speed and simplicity.

Just install the package, add the config and it is ready to use!

## Requirements

* PHP >= 5.6.0
* PDO Extension

## Installation

    composer require memran/marwadb:dev-master

## Usage

    //create Database Class

    require_once('../vendor/autoload.php');
    use MarwaDB\Connection;
    use MarwaDB\DB;

    //Database Configuration
    $config = [
    			'default'=>
    				[
    					'driver' => "mysql",
    					'host' => "localhost",
    					'port' => 3306,
    					'database' => "rbc",
    					'username' => "root",
    					'password' => "",
    					'charset' => "utf8mb4",
    				],
    			'sqlSrv'=>
    				[
    					'driver' => "mysql",
    					'host' => "localhost",
    					'port' => 3306,
    					'database' => "rbc",
    					'username' => "root",
    					'password' => "",
    					'charset' => "utf8mb4",
    				]
    			];

    //database Class Object
    $db = new DB($config);

    //DB Query
    $result = $db->rawQuery('SELECT * FROM system WHERE id = ?',[1]);

    //symfony varDump Function to print result
    dump("Total Rows Returned >>> ".$db->count());
    dump($result);

    //Connection name Specified Query
    $result=$db->connection('sqlSrv')->rawQuery('SELECT * FROM system WHERE id = ?',[1]);
  	dump("Result >>> ".$result->username);

    //Default Connection name
    $result=$db->connection()->rawQuery('SELECT * FROM system WHERE id = ?',[1]);
   	dump("Result >>> ".$result->username);

    //Select All
    $result=$db->select('SELECT * FROM system');
    dump("Result >>> ".$result[0]->username);

    //Select Query with Where
    $result=$db->select('SELECT * FROM system WHERE id = ?',[1]);
    dump("Result>>> ".$result->username);

    //SELECT single row query
    $result = $db->rawQuery("SELECT * FROM system WHERE id = :id LIMIT 1", ['id' => '1']);
    dump("Single Row Result >> ".$result->username);

    //query to fetch all data
    $result = $db->table('system')->select("username","password")->get();
    dump($result);

    //get all results
    $result = $db->table('system')->select("*")->get();
    dump($result);

    //get all result
    $result = $db->table('system')->select()->get();
    dump($result);

    $result = $db->table('system')->select("username")->addSelect("password")->get();
    dump($result);

    //loop throgh result set
    	foreach($result as $row )
    	{
    		dump("Username :".$row->username);
    	}

    //Query Builder on Where condition
    $result=$db->table('system')->select()->whereRaw('id = ?',["1"])->get();
    dump($result);

    //Query Builder Select Query
    $result=$db->table('system')->select()->whereRaw('id = 1')->get();
    dump($result);
    //query for whereRaw and orWhereRaw Condition together
    $result=$db->table('system')->select()->whereRaw('id = ?',['1'])->orWhereRaw('id = ?',['2'])->get();
    dump($result);

    //query for whereRaw and andWhereRaw Condition
    $result=$db->table('system')->select()->whereRaw('id = ?',[1])->andWhereRaw('username = ?',['admin'])->get();
    dump($result);

    //where two column paramenter
    $users=$db->table("system")->select()->where("id","1")->get();
    dump($users);

    //where two column paramenter
    $users = $db->table("system")->whereExists(function() use($db)
    {
    	return $db->table('rule')->select()->sqlString();
    });
    dump($users);

    //query for orderby, offset, limit, groupby
    $result = $db->table('system')->select()->orderBy('id','DESC')->offset(0)->limit(1)->groupBy('id')->get();
    dump($result);

    //query for havingRaw
    $result=$db->table('system')->select()->havingRaw('id = 1')->get();
    dump($result);

    //select Raw
    $result=$db->table('system')->select("username")->get();
    dump($result);

    //select column by choose
    $result=$db->table('system')->select("*")->get();
    dump($result);

    //select distinct
    $users = $db->table('system')->distinct("username")->get();
    dump($users);

    //inner join
    $users=$db->table("system")->select()->join("rule",'rule.id','=','system.type')->get();
    dump($users);

    //leftjoin Sql
    $users=$db->table("system")->select()->leftjoin("rule",'rule.id','=','system.type')->get();
    dump($users);

    //rightJoin
    $users=$db->table("system")->select()->rightjoin("rule",'rule.id','=','system.type')->get();
    dump($users);

    //whereBetween
    $users=$db->table("system")->select()->whereBetween('id',[0,10])->get();
    dump($users);

    $users=$db->table("system")->select()->whereBetween('id',[0,10])->orWhereBetween('id',[0,10])->get();
    dump($users);

    //where is NULL
    $users=$db->table("system")->select()->whereNull('id')->get();
    dump($users);

    //where is NOT NULL
    $users=$db->table("system")->select()->whereNotNull('id')->get();
    dump($users);

    //Count Total Rows
    $users=$db->table("system")->count('*','total');
    dump($users);

    //sum of column
    $users=$db->table("system")->sum('id','total');
    dump($users);

    //Wherein
    $users=$db->table("system")->select()->whereIn("id",[1,2])->get();
    dump($users);

    //WhereNotIn
    $users=$db->table("system")->select()->whereNotIn("id",[1])->get();
    dump($users);

    //WhereDate
    $users=$db->table("activecalls")->select()->whereDate("connecttime",'2019-06-17')->get();
    dump($users);

    //whereMonth
    $users=$db->table("activecalls")->select()->whereMonth("connecttime",'06')->get();
    dump($users);

    //insert and save thedata
     $result=$db->table('rule')->insert(
     		[
    	 			['rtype' => 'Operator6', 'permission' => 'Administrator'],
    	 			['rtype' => 'Operator8', 'permission' => 'Administrator'],
    	 		]
    	 )->save();
    dump($result);

    //insert and retrieve last inserted id
        $result=$db->table('rule')->insertAndGetId(
     		[
     			['rtype' => 'Operator7', 'permission' => 'Administrator'],
     			['rtype' => 'Operator9', 'permission' => 'Administrator'],
     		]
     );
     dump($result);

    //update the data
    $result = $db->table('rule')->update(
    			['rtype' => 'Operator1', 'permission' => 'Administrator']
    )->where("id","117")->save();

    dump($result);

    $result = $db->table('rule')->update(
     			['rtype' => 'Operator2', 'permission' => 'Administrator']
     )->where("id","109")->save();

    dump($result);

    $result = $db->table('rule')->delete()->where("id","118")->save();
    dump($result);

