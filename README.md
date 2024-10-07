# tokokita

Nama    : Rassya Hafizh Suharjo

NIM     : H1D022068

Shift Baru : A

Shift Lama : D

## screenshot dan penjelasan serta kode

### Registrasi
1. Tampilan page registrasi = registrasi_page.dart
Page registrasi terdiri atas 3 texteditingcontroller yang kemudian dibuat menjadi 4 textfield untuk memasukkan nilai dan 1 elevatedbutton untuk tombol submit.
``` dart
final _formKey = GlobalKey<FormState>();
  bool _isLoading = false;
  final _namaTextboxController = TextEditingController();
  final _emailTextboxController = TextEditingController();
  final _passwordTextboxController = TextEditingController();

body: SingleChildScrollView(
        child: Padding(
          padding: const EdgeInsets.all(8.0),
          child: Form(
            key: _formKey,
            child: Column(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                _namaTextField(),
                _emailTextField(),
                _passwordTextField(),
                _passwordKonfirmasiTextField(),
                _buttonRegistrasi()
```
![registrasi](gambar/registrasi.jpg)

2. Registrasi berhasil dan gagal = registrasi_page.dart -> registrasi_bloc.dart
Jika memasukkan value ke TextField dan menekan registrasi, maka widget buttonRegistrasi akan memanggil method submit yang mengirimkan value nama, email, dan password ke registrasi_bloc.dart. jika registrasi berhasil maka akan memanggil showdialog SuccesDialog, sedangkan bila error/gagal akan memanggil WarningDialog
``` dart
Widget _buttonRegistrasi() {
    return ElevatedButton(
        child: const Text("Registrasi"),
        onPressed: () {
          var validate = _formKey.currentState!.validate();
          if (validate) {
            if (!_isLoading) _submit();
          }
        });
  }

  void _submit() {
    _formKey.currentState!.save();
    setState(() {
      _isLoading = true;
    });
    RegistrasiBloc.registrasi(
            nama: _namaTextboxController.text,
            email: _emailTextboxController.text,
            password: _passwordTextboxController.text)
        .then((value) {
      showDialog(
          context: context,
          barrierDismissible: false,
          builder: (BuildContext context) => SuccessDialog(
                description: "Registrasi berhasil, silahkan login",
                okClick: () {
                  Navigator.pop(context);
                },
              ));
    }, onError: (error) {
      showDialog(
          context: context,
          barrierDismissible: false,
          builder: (BuildContext context) => const WarningDialog(
                description: "Registrasi gagal, silahkan coba lagi",
              ));
    });
    setState(() {
      _isLoading = false;
    });
  }
```

kemudian pada registrasi_bloc.dart data dikirimkan ke restful api menggunakan var apiUrl pada api_url.dart sehingga bisa masuk ke database.
``` dart
class RegistrasiBloc {
  static Future<Registrasi> registrasi(
      {String? nama, String? email, String? password}) async {
    String apiUrl = ApiUrl.registrasi;
    var body = {"nama": nama, "email": email, "password": password};
    var response = await Api().post(apiUrl, body);
    var jsonObj = json.decode(response.body);
    return Registrasi.fromJson(jsonObj);
  }
}
```

``` dart
class ApiUrl {
  static const String baseUrl = 'http://10.99.4.182:8080'; //diganti lagi ke yg pdf ga pake punya sendiri wkwkwk
  static const String registrasi = baseUrl + '/registrasi';
```
![registrasi sukses](gambar/registrasi-sukses.jpg)
![registrasi gagal](gambar/registrasi-gagal.jpg)


### Login
1. Halaman login = login_page.dart

![login](login.jpg)

### produk_page.dart
![List Produk](list_produk.jpg)

### produk_detail.dart
![Detail Produk](detail_produk.jpg)

### produk_form.dart
#### Tambah Produk => produk_page
![Tambah Produk](tambah_produk.jpg)
#### Ubah Produk => produk_detail
![Ubah Produk](ubah_produk.jpg)