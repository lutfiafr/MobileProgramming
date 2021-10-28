1. Membuat aplikasi flutter pemula

edit lib/main.dart , tempat kode Dart berada.

Ganti isi lib/main.dart.
Hapus semua kode dari lib/main.dart . Ganti dengan kode berikut, yang menampilkan "Hello World" di tengah layar.

 	import 'package:flutter/material.dart';

       void main() => runApp(MyApp());

      class MyApp extends StatelessWidget {
        @override
        Widget build(BuildContext context) {
          return MaterialApp(
            title: 'Welcome to Flutter',
            home: Scaffold(
              appBar: AppBar(
                title: const Text('Welcome to Flutter'),
              ),
              body: const Center(
                child: Text('Hello World'),
              ),
            ),
          );
        }
      }
  
lalu kita akan menggunakan external paket Pada langkah ini, Anda akan mulai menggunakan paket sumber terbuka bernama english_words , yang berisi beberapa ribu kata bahasa Inggris yang paling sering digunakan ditambah beberapa fungsi utilitas.Anda dapat menemukan english_wordspaket tersebut, serta banyak paket open source lainnya, di pub.dev .
The pubspec.yamlberkas mengelola aset dan dependensi untuk aplikasi Flutter. Di pubspec.yaml, tambahkan english_words (3.1.5 atau lebih tinggi) ke daftar dependensi:


Pada langkah ini, Anda akan menambahkan widget stateful, RandomWords, yang membuat Statekelasnya, _RandomWordsState. Anda kemudian akan menggunakan RandomWordssebagai anak di dalam MyAppwidget stateless yang ada .

1. Buat kode boilerplate untuk widget stateful.
Di lib/main.dart, posisikan kursor Anda setelah semua kode, masukkan Return beberapa kali untuk memulai pada baris baru. Di IDE Anda, mulailah mengetik stful. Editor menanyakan apakah Anda ingin membuat Statefulwidget. Tekan Kembali untuk menerima. Kode boilerplate untuk dua kelas muncul, dan kursor diposisikan bagi Anda untuk memasukkan nama widget stateful Anda.

2. Masukkan RandomWordssebagai nama widget Anda.
The RandomWordswidget tidak sedikit yang lain di samping menciptakan nya Statekelas.

Setelah Anda memasukkan RandomWordsnama widget stateful, IDE secara otomatis memperbarui Statekelas yang menyertainya , menamainya _RandomWordsState. Secara default, nama Statekelas diawali dengan underbar. Awalan pengenal dengan garis bawah memberlakukan privasi dalam bahasa Dart dan merupakan praktik terbaik yang direkomendasikan untuk Stateobjek.

IDE juga secara otomatis memperbarui kelas status ke extend State<RandomWords>, yang menunjukkan bahwa Anda menggunakan State kelas generik yang dikhususkan untuk digunakan denganRandomWords. Sebagian besar logika aplikasi berada di sini—ini mempertahankan status RandomWordswidget. Kelas ini menyimpan daftar pasangan kata yang dihasilkan, yang tumbuh tanpa batas saat pengguna menggulir dan, di bagian 2 lab ini, pasangan kata favorit saat pengguna menambahkan atau menghapusnya dari daftar dengan mengalihkan ikon hati.

  membuat statefull widget

	class MyApp extends StatelessWidget {
	    @override
	    Widget build(BuildContext context) {
      	      final wordPair = WordPair.random();
	      return MaterialApp(
	        title: 'Welcome to Flutter',
	        home: Scaffold(

	            title: const Text('Welcome to Flutter'),
	          ),
	          body: Center(
            child: Text(wordPair.asPascalCase),
            child: RandomWords(),
	          ),
	        ),
	      );
	    }

Langkah 4: Buat ListView bergulir tak terbatas
Pada langkah ini, Anda akan memperluas _RandomWordsStateuntuk menghasilkan dan menampilkan daftar pasangan kata. Saat pengguna menggulir daftar (ditampilkan dalam ListViewwidget) tumbuh tanpa batas. ListView's builderpabrik konstruktor memungkinkan Anda untuk membangun sebuah tampilan daftar malas, pada permintaan.

2. Lanjutan dari program sebelumnya, menambahkan icon pada setiap nama random yang muncul, menambahkan class _saved sebagai tempat menampung nama favorit
	
class _RandomWordsState extends State<RandomWords> {
	final _suggestions = <WordPair>[];
	final _saved = <WordPair>{};     // NEW
	final _biggerFont = TextStyle(fontSize: 18.0);
	 ...
}
   Menambahkan class alreadySaved agar memastikan nama yang sudah favorit tidak dapat difavoritkan lagi
	Widget _buildRow(WordPair pair) {
	  final alreadySaved = _saved.contains(pair);  // NEW
	  ...
	}
   Menambahkan listTitle berisi bentuk icon hati sebagai tanda favorit
	Widget _buildRow(WordPair pair) {
	  final alreadySaved = _saved.contains(pair);
	  return ListTile(
	    title: Text(
	      pair.asPascalCase,
	      style: _biggerFont,
	    ),
	    trailing: Icon(   // NEW from here... 
	      alreadySaved ? Icons.favorite : Icons.favorite_border,
	      color: alreadySaved ? Colors.red : null,
	      semanticLabel: alreadySaved ? 'Remove from saved' : 'Save',
	    ),                // ... to here.
	  );
	}
	
  Jika app di reload maka akan dapat dilihat bedanya sekarang sudah ada icon hati
  Membuat icon hati dapat berinteraksi ketika disentuh 
	
	Widget _buildRow(WordPair pair) {
	  final alreadySaved = _saved.contains(pair);
	  return ListTile(
	    title: Text(
	      pair.asPascalCase,
	      style: _biggerFont,
	    ),
	    trailing: Icon(
	      alreadySaved ? Icons.favorite : Icons.favorite_border,
	      color: alreadySaved ? Colors.red : null,
	      semanticLabel: alreadySaved ? 'Remove from saved' : 'Save',
	    ),
	    onTap: () {      // NEW lines from here...
	      setState(() {
		if (alreadySaved) {
		  _saved.remove(pair);
		} else { 
		  _saved.add(pair); 
		} 
	      });
	    },               // ... to here.
	  );
	}
	
  Sekarang jika disentuh maka icon hati akan berubah warna merah tanda item sudah difavoritkan.
  Membuat aplikasi dapat berpindah dengan "navigator" dengan kode dibwh ini
	
	class _RandomWordsState extends State<RandomWords> {
	  ...
	  @override
	  Widget build(BuildContext context) {
	    return Scaffold(
	      appBar: AppBar(
		title: Text('Startup Name Generator'),
		actions: [
		  IconButton(
		    icon: const Icon(Icons.list),
		    onPressed: _pushSaved,
		    tooltip: 'Saved Suggestions',
		  ),
		],
	      ),
	      body: _buildSuggestions(),
	    );
	  }
	  ...
	}
  Menambahakan list favorit sebagai tempat item item favorit
	void _pushSaved() {
  }
	void _pushSaved() {
	  Navigator.of(context).push(
	  );
	}
 Reload aplikasi maka sekarang sudah bisa dilihat item favoritnya

 Membuat tema untuk aplikasi
	
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Startup Name Generator',
      theme: ThemeData(          // Add the 5 lines from here... 
        appBarTheme: const AppBarTheme(
          backgroundColor: Colors.white,
          foregroundColor: Colors.black,
        ),
      ),                         // ... to here.
      home: RandomWords(),
    );
  }
}
	
Reload maka aplikasi sudah dapat diganti temanya