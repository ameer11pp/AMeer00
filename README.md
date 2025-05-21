import binascii

def patch_binary_file(file_path, offset_hex, new_bytes_hex):
    """
    تطبق باتش على ملف ثنائي.

    Parameters:
    file_path (str): المسار إلى الملف الثنائي (مثل "libanogs.so").
    offset_hex (str): الإزاحة كـ string سداسي عشري (مثل "0x0000").
    new_bytes_hex (str): البايتات الجديدة كـ string سداسي عشري (مثل "00 00 A0 E3").
    """
    try:
        # تحويل الإزاحة من string سداسي عشري إلى عدد صحيح
        offset = int(offset_hex, 16)

        # تنظيف string البايتات الجديدة وإزالة المسافات
        cleaned_bytes_hex = new_bytes_hex.replace(" ", "")
        
        # تحويل string البايتات السداسية العشرية إلى بايتات فعلية
        # يجب التأكد من أن طول الـ string زوجي
        if len(cleaned_bytes_hex) % 2 != 0:
            print(f"خطأ: طول string البايتات الجديدة غير زوجي. {new_bytes_hex}")
            return

        new_bytes = binascii.unhexlify(cleaned_bytes_hex)

        with open(file_path, 'rb+') as f:
            # الانتقال إلى الإزاحة المطلوبة
            f.seek(offset)
            # كتابة البايتات الجديدة
            f.write(new_bytes)
        
        print(f"تم تطبيق الباتش بنجاح على {file_path} عند الإزاحة {offset_hex}.")

    except FileNotFoundError:
        print(f"خطأ: الملف {file_path} غير موجود.")
    except ValueError as e:
        print(f"خطأ في التحويل: {e}")
    except Exception as e:
        print(f"حدث خطأ غير متوقع: {e}")

# مثال على الاستخدام:
# تأكد من أن ملف libanogs.so موجود في نفس مسار السكربت أو قم بتحديد المسار الكامل له.
# أنشئ ملفًا تجريبيًا لتجربة الباتش عليه
with open("libanogs.so", "wb") as f:
    f.write(b'\x00' * 100) # ملف بحجم 100 بايت كله أصفار للتجربة

patch_binary_file("libanogs.so", "0x0000", "00 00 A0 E3 1E FF 2F E1")

# لتأكيد التغييرات، يمكنك قراءة الملف بعد التعديل
# with open("libanogs.so", "rb") as f:
#     f.seek(0x0000)
#     print(binascii.hexlify(f.read(8))) # قراءة 8 بايتات من الإزاحة 0x0000
