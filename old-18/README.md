## old - 18

```php

if($_GET['no']){
  $db = dbconnect();
  if(preg_match("/ |\/|\(|\)|\||&|select|from|0x/i",$_GET['no'])) exit("no hack");
  $result = mysqli_fetch_array(mysqli_query($db,"select id from chall18 where id='guest' and no=$_GET[no]")); // admin's no = 2

  if($result['id']=="guest") echo "hi guest";
  if($result['id']=="admin"){
    solve(18);
    echo "hi admin!";
  }
}
```

- Bài này sqli 
>if(preg_match("/ |\/|\(|\)|\||&|select|from|0x/i",$_GET['no'])) exit("no hack");
Lọc các kí tự hay dùng để injection.
>$result = mysqli_fetch_array(mysqli_query($db,"select id from chall18 where id='guest' and no=$_GET[no]")); // admin's no = 2
Dòng này có gợi ý admin là no2, câu query của nó là where id='guest' and no=$_GET[no], mà theo ta có

TRUE and TRUE => TRUE
TRUE and FALSE => FALSE
FALSE or TRUE => TRUE
(TRUE and FALSE) or TRUE => TRUE

thế nên: id=guest and no=-1 or no=2 =>  (true and false) or true

ok, tiếp theo là encode url để tránh bộ lộc, ta dùng %09 = tab => -1%09or%09no=2