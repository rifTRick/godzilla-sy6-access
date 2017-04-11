BUSET NGAPAIN LOE AMPE KESINI-SINI NYARI GUE:

<input type="text" placeholder="Nama Keren Loe" id="nama"><br>
<input type="text" placeholder="Password" id="umur"><br>
<input type="submit" value="TGet Access" onclick="add()">
<div id="out"></div>

<script type='text/javascript'>
//javascript quote in arrays db
quote = new Array(5);
quote[0] = "<b>khusu orang tampan</b>";
quote[1] = "<b>apakah anda yakin tampan</b>";
quote[2] = "<b>menurut kami anda tidak tampan</b>";
quote[3] = "<b>silahkan anda berkaca</b>";
quote[4] = "<b>kembali kesini jika anda sudah tampan</b>";
//menentukan random index
index = Math.floor(Math.random() * quote.length);
//menampilkan quote
document.write("\n");
document.write(quote[index]);
//db end arrays
</script>
