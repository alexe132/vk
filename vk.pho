<?php
 
if (!isset($_REQUEST)) {
    return;
}
 
//Строка для подтверждения адреса сервера из настроек Callback API
$confirmationToken =  '5c41b4de';
 
//Ключ доступа сообщества
$token = 'vk1.a.ng6haRuMVZbwgRgxOukCW-HmffFQPsVVwBbJ8E-JwUgjTb2Z-xUN8LZ0fhfBkxaAuGqd2VEZpQfBFz4Ltu-Z1CZ8ypO04cpVA5xxmGB3vwO77kZeZqIskX4ahB_XETkYi7_Xev6wVqxVCChOGRU-Tha3PIkULR2dB5nvX5n6hcZ426Oq6XMcW-KRYl0I9a8u';
 
// Secret key
$secretKey = 'Levina1993';
 
//Получаем и декодируем уведомление
$data = json_decode(file_get_contents('php://input'));
 
// проверяем secretKey
if(strcmp($data->secret, $secretKey) !== 0 && strcmp($data->type, 'confirmation') !== 0)
    return;
 
//Проверяем, что находится в поле "type"
switch ($data->type) {
    //Если это уведомление для подтверждения адреса сервера...
    case 'confirmation':
        //...отправляем строку для подтверждения адреса
        echo $confirmationToken;
        break;
 
    //Если это уведомление о новом сообщении...
    case 'message_new':
        //...получаем id его автора
        $userId = $data->object->user_id;
        //затем с помощью users.get получаем данные об авторе
        $userInfo = json_decode(file_get_contents("https://api.vk.com/method/users.get?user_ids={$userId}&v=5.0"));
 
        //и извлекаем из ответа его имя
        $user_name = $userInfo->response[0]->first_name;
 
        //С помощью messages.send и токена сообщества отправляем ответное сообщение
        $request_params = array(
            'message' => "{$user_name}, добрый день!<br>".
                            "Отвечу Вам в ближайшее время.",
            'user_id' => $userId,
            'access_token' => $token,
            'v' => '5.0'
        );
 
        $get_params = http_build_query($request_params);
 
        file_get_contents('https://api.vk.com/method/messages.send?' . $get_params);
 
        //Возвращаем "ok" серверу Callback API
        echo('ok');
 
        break;
 
    // Если это уведомление о вступлении в группу
    case 'group_join':
        //...получаем id нового участника
        $userId = $data->object->user_id;
 
        //затем с помощью users.get получаем данные об авторе
        $userInfo = json_decode(file_get_contents("https://api.vk.com/method/users.get?user_ids={$userId}&v=5.0"));
 
        //и извлекаем из ответа его имя
        $user_name = $userInfo->response[0]->first_name;
 
        //С помощью messages.send и токена сообщества отправляем ответное сообщение
        $request_params = array(
            'message' => "{$user_name}, добрый день!<br>" .
                            "Спасибо за подписку на мое сообщество.<br>" .
                            " <br>" .
                            "Дарю Вам скидку за подписку - 500 руб. на первое посещение.<br>" .
                            "Ей можно воспользоваться, если общая стоимость процедур, без учета скидки, будет от 2 000 руб. <br>" .
                            " <br>" .
                            "Посмотреть список процедур можно тут - https://vk.com/cg4YLf" .
                            "Записаться тут - https://vk.com/cg1HaB",
            'user_id' => $userId,
            'access_token' => $token,
            'v' => '5.0'
        );
 
        $get_params = http_build_query($request_params);
 
        file_get_contents('https://api.vk.com/method/messages.send?' . $get_params);
 
        //Возвращаем "ok" серверу Callback API
        echo('ok');
 
        break;
}
?>
