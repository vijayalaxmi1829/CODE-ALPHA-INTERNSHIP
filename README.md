<!DOCTYPE html>
<html>
<head>
    <title>Music Recommendation System</title>
</head>
<body>
    <h1>Music Recommendation System</h1>
    <form id="musicForm">
        <label for="musicId">Enter Music ID:</label>
        <input type="text" id="musicId" name="musicId">
        <button type="submit">Recommend Music</button>
    </form>
    <div id="recommendations"></div>

    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <script>
        $(document).ready(function() {
            $('#musicForm').submit(function(event) {
                event.preventDefault();
                var musicId = $('#musicId').val();
                $.ajax({
                    type: 'POST',
                    url: '/recommend',
                    contentType: 'application/json',
                    data: JSON.stringify({ music_id: musicId }),
                    success: function(response) {
                        $('#recommendations').empty();
                        $.each(response, function(index, music) {
                            $('#recommendations').append('<p>' + music['title'] + ' by ' + music['artist'] + ' - ' + music['genre'] + '</p>');
                        });
                    }
                });
            });
        });
    </script>
</body>
</html>
