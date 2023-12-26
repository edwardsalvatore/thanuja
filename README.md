<?php
header("Access-Control-Allow-Origin: *");
header("Access-Control-Allow-Methods: GET, POST, OPTIONS");
header("Access-Control-Allow-Headers: Content-Type");

if ($_SERVER['REQUEST_METHOD'] === 'OPTIONS') {
    http_response_code(200);
    exit();
}

if ($_SERVER['REQUEST_METHOD'] === 'GET') {
    $url = isset($_GET['url']) ? $_GET['url'] : '';
    
    if (filter_var($url, FILTER_VALIDATE_URL)) {
        $response = makeRequest($url);
        echo json_encode($response);
    } else {
        http_response_code(400);
        echo json_encode(['error' => 'Invalid URL']);
    }
} else {
    http_response_code(405);
    echo json_encode(['error' => 'Method Not Allowed']);
}

function makeRequest($url) {
    $options = [
        'http' => [
            'method' => 'GET',
        ],
    ];

    $context = stream_context_create($options);
    $result = file_get_contents($url, false, $context);

    if ($result === false) {
        return ['status' => 'error', 'message' => 'Request failed'];
    }

    return ['status' => 'success', 'message' => 'Request successful'];
}
?>
