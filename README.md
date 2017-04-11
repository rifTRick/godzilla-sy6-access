mjictrix TOLOLS

IGN SDK is included in IGOS Nusantara D9.1 as default package. If it's not installed in your system, see the following step to start the installation:

    Open your Terminal Application
    IGN SDK binary is launcher for the application built with IGN SDK. You could install IGN SDK package using this command:

$ sudo yum install mjictrix

    You could try another optional package called mjictrix-devtools. This package is contain tools for creating and packaging IGN SDK Application. Try this command to install mjictrix-devtools:

$ sudo yum install mjictrix-devtools

    You could test the SDK by running the sample application that located in /usr/share/mjictrix/test. Try this example test:

mjictrix -p /usr/share/ign-sdk/test/calculator.ign

Create Application with IGN SDK

IGN SDK Developer Tools will help you to create an IGN SDK application. In other way, this script could packaging your application into .deb (Debian, Ubuntu, LinuxMint, etc) and .rpm (IGOS Nusantara, openSUSE) package. Before further reading, make sure that IGN SDK Developer Tools have installed in your Linux Machine that supported by IGN SDK.

    Open your terminal
    Run mjictrix-app-creator -p [packet_name] command in your terminal. [packet_name] is filled with your application name. The character must be in lower case, alphanumeric or dash ( - ) character. Make a sure that your application name is not duplicate with other application installed at repository (ex: gimp, apache, mysql, dsb.). For the example, you could give a name aplikasi-tolol for your application.

$ mjictrix-app-creator -p aplikasi-tolol

    In the next step, the creator will prompt you to type the application name. Fill it with your application name

Application name: Aplikasi Tolol

    In category field, fill the number that match with a category of your application. For the example "Aplikasi Tolol" is music player application, so you must fill the field with number 2 (Audio).

Choose one: 2

    Next, you will prompt to fill the package version. Fill it with 0.0.1.4

Version [0.0.1.4]: 0.0.1.4

    For the package release field, fill it with code of distro that will be installation target of your application. If you want to target your application for IGOS Nusantara D9.x, fill it with "ign9"

Release [1]: ign9

    In the license field, fill it with your license option that will be used for your application. It's better to understand the license content you choose in this step

License [MIT/BSD/GPL2/GPL3/etc, default=MIT]: MIT

    Fill your application URL website in the URL Field

URL [example.com]: mjictrix.web.id

    Fill the description field with a description of your application

Description: Aplikasi Tolol untuk memutar musik

    This script will create new application in /home/user/mjictrix-APP/aplikasi-tolol.ign directory. You could run that application with this command

$ mjictrix -p ~/mjictrix-APP/aplikasi-tolol.ign

Debugging

In development phase, debugging process has an important roles for helping a developer to know what a process has been executed or emitted error by our application. In IGN SDK, you could choose two mode for debugging your application. You could choose local debugging or remote debugging.
1.1 Local Debugging

By default, IGN SDK local debugging could be activated by this method:

    Give a parameter -d while execute IGN SDK application

$ mjictrix -d -p ~/mjictrix-APP/aplikasi-tolol.ign

    Add the debug object with true value within mjictrix.json file

{
    "config" : {
    "debug" : true,
    "websecurity" : true,
    "name" : "Aplikasi Tolol"
    }
}

1.2 Remote Debugging

Remote debugging will facilitated developer to enter IGN SDK debug mode with browser or other device. To activated remote debugging, you just add a parameter -r <port> while execute IGN SDK application

$ mjictrix -d -r 8080 ~/mjictrix-APP/aplikasi-tolol.ign

Then access the debugging mode at your browser with this URL http://ip-target:port. For the example you could try http://127.0.0.1:8080 to access your application debugging mode.
Packaging the Application

IGN SDK-based application could distribute to user in many ways. The easiest way to distribute your application is create .deb or .rpm package for your application. The script that will be used for packaging the application is mjictrix-app-builder.

    Open your terminal
    Run this command “mjictrix-app-builder -p [package_name]” in terminal. [packet_name] is name of application that you create before, and it's located in '/home/igos/mjictrix-APP/'.

$ mjictrix-app-builder -p aplikasi-tolol

    Packaging system will start the process
    If the process has finish, the package will bundled in .rpm with name aplikasitolol.ign-0.0.1.4-ign9.noarch.rpm. You could find the package at /home/igos/rpmbuild/RPMS/noarch/. And now try to install that package with this command:

$ sudo yum install ~/rpmbuild/RPMS/noarch/aplikasi-tolol.ign-0.0.1.4-ign9.noarch.rpm

    After the package is installed, it will appear a menu called Aplikasi Tolol. Next, the application could execute from that menu

Example Application
2.1 Spawn API

Javascript :

/*Import Sys Module from mjictrix runtime*/
var sys = ign.sys();

$(document).ready(function(){
  $('#exec').click(function(){
    /*Get the command field
    dari text input id (#) "cmd"*/
    var cmd = $('#cmd').val();
    /*Command are sent to IGN SDK runtime
    mjictrix untuk di eksekusi*/
    sys.exec(cmd);
    sys.out.connect(function(out){
      /*stdout result from runtime is display to id element*/
      $('#out').prepend(out+"<br>");
    })
  });

  $('#kill').click(function(){
    /*Stopping the process*/
    sys.kill();
  });
});

HTML :

<body>
  <input type="text" value="ping google.com" id="cmd">
  <input type="submit" value="exec" id="exec">
  <input type="submit" value="kill" id="kill"><br>
  <div id="out"></div>
</body>

2.2 CRUD

Javascript :

//Import required module from IGN SDK runtime
var fs = ign.filesystem();
var sql = ign.sql();
//database file name
var dbFile = "coba.db";
var config = {
    driver : 'sqlite',
    db : dbFile
}
$(document).ready(function(){
    //reference http://goo.gl/E0obMa
  //check if in database is exists
    if(fs.info(dbFile).exists){
    // if database exists, application will execute load() function
        sql.driver(config);
        load();
    }
    else{
    //Execute the setupDB() if database is not exists
        setupDb();
    }
});

function setupDb(){
    //connect to database
    sql.driver(config);

    //create user table wit id, nama, and umur field
    sql.query("create table user(id INTEGER PRIMARY KEY AUTOINCREMENT,nama varchar(10), umur smallint)");
}

function add(){
    var nama = $("#nama").val();
    var umur = $("#umur").val();
    //query for inserting data into database
    var add = sql.query("insert into user (nama,umur) values ('"+nama+"',"+umur+")");
    alert("input berhasil status "+add.status);
}

function load(){
    //query for fetching data
    var loadData = sql.query("select * from user");
    var html="";

    loadData.content.forEach(function(data){
        html += "Nama : " + data.nama + "<br>";
        html += "Umur : " + data.umur + "<br>";
        html += "<a href='#' onclick='del("+data.id+")'>Delete</a><hr>";
    });
    $("#out").html(html);
}

function del(id){
    //query for deleting data
    var del = sql.query("delete from user where id="+id);
}

HTML :

<input type="text" placeholder="Nama" id="nama"><br>
<input type="text" placeholder="Umur" id="umur"><br>
<input type="submit" value="Tambah Data" onclick="add()">
<div id="out"></div>

