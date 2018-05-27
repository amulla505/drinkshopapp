All asset for this repository : https://drive.google.com/drive/u/0/mobile/my-drive?usp=sharing




Hello, everybody I will show you how to write drink shop app with PHP backend.
And firebase. in this part we will learn how to use firebase account kit to authentication user and register account demo.

First, we need to create new project in Android 





Next add library to use 



dependencies { 

implementation fileTree (d1: : o -w 'libs', include: [".jar‘]) 

implementation 'com. android. support: appoompat v7 :27 . 1 . 1 ' 

implementation ' oom. android. :upport . constraint : constraint 1:yout:1 . 1 . 0 ' 

testImplementation ’ junit: junitz4 . 12 ' 

androidTestImplementation ' com. android. support . tust : runner : 1 . 0 . 2 ' android’l‘estImplementation 'oom. android. support . test. osprasso f osprasso acre :3 . D . 2' 

android. support : cardview v7 : 27 . 1 . 1 ' 

android. support : design : 27 . 1 . 1 ' rengwuxian .matorialedittext : library : 2 . 1 . 4' :zagurskii :pattarnedtextwatchar : 0 . 5 . O ' 

.github.d manuspots dialog: 0. 7@aar' 

squaroup . zetrofit2 : retrofit : 2 . 3 . 0 ' squareup. retrofitz : oonv-r'ter gson : 2 . 3 . 0' 

}



Now we need to create new database on MYSQL use PHPmyadmin and write webservice backend.


photo

After create new database and new table we will add primary key for phone column now we will write webservices for this project. In this tutorial we will write 2 methods

- Check Exist User
- Register New User


Config.php

<?php 

/* * Database Config Information */ 

define ( "DB__EOS'1'" , " localhost“) 7 define ( "DB_USER" , "root") ; 

define ("DB__PASSWORD" , " ") ; I define-("DB_DATABASE", "drinkshop) ;
 ?> 



db_conn.php


< ?php 

1 

class DB_Connect( private $conni 

'.’> 

public function connect () 

( 

raquire_onca ‘ config . php ' 

This >conn new mysqli( host: DE_HOST, username: DB_USER, passwd. DE_PASSWORD, dbname: DB__DATABASE) ,return this >conn

 }
}
?>



db_functions.php




<?php 

class DB_Functions( private $conn: 

function __construct () 

I require_once ’ db_connect .php ' ; 

Sdb = new DB_Connect() : $this->conn = $db->connect() : 

function _destruct () 

l I // T000: Implement _destruct () method. 

/» * Check user exists * return true/false */ 

function checkExistsUser ($phone) 

{ 

{ $stmt = $this->conn->prepare("SELECT * FROM Use: WHERE Phone=?"): $3tmt->bind_param(“s",Sphone); $5tmt->execute(); $3tmt->store_;result(): 

if($stmt->num_;ows > 0) 

{ $stmt->close(): return true; 

) 

else( 

$5tmt->close(): return false



/* * Register new user * return User object if user was created * return error message if have exception */ public function registerNewUser($phone,$name,$birthdate,$address) 

( Stmt = $this->conn->prepare("INSERT INTO User(Phone,Name,Birthdate,Address) VALUES(?,?,?,?)"): 

Stmt->bind_param("ssss",$phone,$name,$birthdate,Saddress)t $result=$stmt->execute(): $3tmt->close()f 

if($result) ( $stmt=$this->conn->prepare("SELECT * IROM User WHERE Phone = ?"): $stmt->bind_peram("s",$phone); $stmt->execute(); $user = $3tmt->get_resu1t()->fetch_assoc(): $5tmt->close(); return $user; } 

else return false; 

/t * Register new user * return User object if user was created 9 * return false and show error message if have exception 

*/ public function registerNewUser ($phone, Shame, $birthdate, $address) 

( $stmt = $this->conn->prepare (" INSERT INTO User (Phone,Name, Birthdate,Address) VALUES (? , '2 , ? , '2) ") : 

$5tmt~>bind_param ( "ssss" , Sphone, $name, Shirthdate, $address) ; 

$result=$3tmt->execute () : $5stmt->close () ; 

i£($result) l $stmt=$this->conn->prepare("SELECT * FROM User WHERE Phone = 9") ' $stmt->bind_param("s", $phone) : I $stmt->execute () .$user = $stmt->get result() _ ->fetch a $stmt->close () : _ 53°C 0 I 

return Suser; } else 

return false; 
      }
}
?> 

checkuser.php

packagedmt . dev . androiddrinkshop ‘ Model ; 

public class CheckUserResponse { private heel-an exists; private string ozzor_msg; 

public CheckUserResponseO I 

,r 

public boolaux isExistsO (I return exists; 

} 

public void setExists(boolean exists) { Humanist: = exists; 

} 

public String getError_msg() ( 

return error msq; } 



And we need to create data model for this response in Android 


Checkpoint.php


packagedmt . dev . androiddrinkshop ‘ Model ; 

public class CheckUserResponse { private heel-an exists; private string ozzor_msg; 

public CheckUserResponseO I 

,r 

public boolaux isExistsO (I return exists; 

} 

public void setExists(boolean exists) { Humanist: = exists; 

} 

public String getError_msg() ( 

return error msq; } 



Register.php

Sresponse = array(): 

if (isset ($_POST [ 'phone' ]) H isset($_POST['nama']) && isset(5_POST['birthdate'1) E5 isset($_POST['address'])) 

Sphone = $_POST['phone']; 

Sname = $_POST['name']: Sbirthdate = $_POST['birthdate']: $address = S P05T['address']; 

if($db->checkExistsUser(Sphone)) 

( $response["error_psg“] = “User already exsted with “.$phone: echo json_encode($response)i 

) 

else 

( 

//CReate new user 

Suser = $db->registerNewUser(Sphone,$name,$birthdate,$address)i if($user) 

( $response["phone“] = $user[“Phone“]; $response[“name“] = $USer[“Name“]; 9 $response["birfhdate"] = $user[“birthdate“]; $regiter[ "Phone“} ) 

else

( 

//CReate new user . _ . Suser = $db->registerNewUser(Sphone,Sname,Sblrthdate,:address). if($user) 

( $re5ponse[“phone"] = $user["Phone“]: $response["name"] = $user[“Name"]: $response["birthdate"] = $user["Birthdate“]; $re5ponse["address"] = $user["Address"]; echo jsoq_encode($response); 

) 

else 

( 

$response["error_psg"] = "Unknow error occurred in registration!“: echo jsoq_encode($response)i 

) ) 

else{ 

$response["error_psg"] = "Re 

required parameter b _ _ _ echo jS°Q_enCode($response); (P one.name,birthdate,address is missing; 

?> 




Yes! We have done to write webservices backend,

 Now we will try use advanced rest client to test

Ok! API work good

Now we need to write user model for Android app 


user.java

package edmt.dev.androiddrinkshop.Model; 

public class User { 
privatString phone; 
private String address; 
private String name; 

privata string bithdate; private String e:error_msg; // It will empty if use: return object 






Now setup retrofit


IDdrinkshop.

import edmt . dev. androiddrinkshop .Model . CheckUserResponse; import edmt . dev. androiddrinkshop .Model . User; 

import retrofitz . Call; 

import retrofitz .http. Field; 

import retrofit-2 . http . FormUrlEncoded; import retrofit-2 .http.POST: 

public interface IDrlnkShopAPI { @FormUrlEncoded @POST ( "checkusaz .php") Ca11<checkUserResponse> checkUserExlsts(@F1e1d("phone") String phone); 

@E‘ormUrlEncoded 

@POST ( "xugistez .php") 

Call<User> registerNewUsermFleld("phone") String phone, @Field("naxn.") String name, @Field("address") String address, @Fied("birthdate") String birthdate) ; 




Retrofit.


import retrofit2.Retrofit; l import retrofitZ.converter.gson.GsonConverterFactory; public class Retrofltcllent { private static Retrofit retrofit=null; 

public static Retrofit getClient (String baseUrl) I 1£(retrofit == null) { retrofit = new Retrofit.Bui1der() .baseUrl (baseUrl) . addconverterEactory (GsonconverterE‘actory. area be ( ) ) .buildO ; } 

return retrofit; 

Common.java


package edmt . dev . androiddrinkshop . Utils ; 

import edmt . dev. androiddrinkshop . Re trofi t: . IDrinkShopAPI; import edmt . dew . androiddrinkshop . Retrofit . RetrofitClient; 

public class Common ( //In Emulator , localhost = 10. 0.2.2 

private static final Strung BASE_W http.//10.0.2.2/drinkshop/”; 

public static IDrinkShopAPI getAPI () { 

return Retrofitclient: . etClient ' } g (BASELM) . create (IDrlnkShopAPI. class) ; 




Now we just design UI for main screen





In next part I will show you how to integrate Facebook account kit to this app and register new account.
