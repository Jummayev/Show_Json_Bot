 define('API_KEY','your bot token');
 
 function ty($ch){ 
        return bot('sendChatAction', [
            'chat_id' => $ch,
            'action' => 'typing',
        ]);
    }
    
    function bot($method,$datas=[]){
        $url = "https://api.telegram.org/bot".API_KEY."/".$method;
        $ch = curl_init();
        curl_setopt($ch,CURLOPT_URL,$url);
        curl_setopt($ch,CURLOPT_RETURNTRANSFER,true);
        curl_setopt($ch,CURLOPT_POSTFIELDS,$datas);
        $res = curl_exec($ch);
        if(curl_error($ch)){
            var_dump(curl_error($ch));
        }else{
            return json_decode($res);
        }
    }
    
    $update = json_decode(file_get_contents('php://input'));
    $message = $update->message;
    $chat_id = $message->chat->id;
    
    ty($chat_id);
    $json = json_encode($update, JSON_PRETTY_PRINT);
    
        bot('sendMessage',[
            'chat_id'=>$chat_id,
            'text'=>"<pre>$json</pre>",
            'parse_mode'=>"html",
                  
        ]);
