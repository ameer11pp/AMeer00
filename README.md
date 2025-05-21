<!DOCTYPE html>
<html>
<head>
    <title>أداة تعديل الأوفست للمكتبات</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            direction: rtl;
        }
        .container {
            display: flex;
            flex-direction: column;
            gap: 15px;
        }
        textarea, input {
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 4px;
        }
        button {
            padding: 10px 15px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }
        .output {
            background-color: #f5f5f5;
            padding: 15px;
            border-radius: 4px;
        }
    </style>
</head>
<body>
    <h1>أداة تعديل الأوفست للمكتبات</h1>
    
    <div class="container">
        <div>
            <label for="library">اسم المكتبة:</label>
            <input type="text" id="library" value="libanogs.so">
        </div>
        
        <div>
            <label for="offset">عنوان الأوفست:</label>
            <input type="text" id="offset" value="0x0000">
        </div>
        
        <div>
            <label for="bytes">البايتات الجديدة (بصيغة hex مفصولة بمسافات):</label>
            <textarea id="bytes" rows="3">00 00 A0 E3 1E FF 2F E1</textarea>
        </div>
        
        <button onclick="generateCode()">إنشاء كود التعديل</button>
        
        <div class="output">
            <h3>الكود الناتج:</h3>
            <pre id="output"></pre>
        </div>
    </div>

    <script>
        function generateCode() {
            const library = document.getElementById('library').value;
            const offset = document.getElementById('offset').value;
            const bytes = document.getElementById('bytes').value;
            
            const code = `PATCH_LIB(XorStr("${library}"), XorStr("${offset}"), XorStr("${bytes}"));`;
            
            document.getElementById('output').textContent = code;
        }
    </script>
</body>
</html>
