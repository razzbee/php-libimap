# PHP-LibImap
PHP libimap is a PHP imap library in pure OOP, the idea is to make the native php_imap functions easier to use with performance tunning in mind.



###Installation
using composer , run :
composer require razzbee/PHP-LIBIMAP


###Usage 

####Initialise class

```php
$imap = new \PHPLibImap\IMAP;
```

**connect an imap server**


```php
$imap->connectServer([
   'host' => 'localhost',
   'port' => 993,
   'imap_username' => 'username@domain.tld',
   'password' => 'secret',
   'mailbox_name' => 'INBOX',
   'enableSSL' => true,
]);
```

**host** : Your Imap mail server hostname , example : localhost , 19.10.10.2 , imap.domain.tld or domain.tld 


**port** : your mailserver's imap's port , the default for secure connection is 993 and no secure connection is 143


**imap_username** : The full qualified email address of mail account , example: you@example.com 


**password** : your mail account password.


**mailbox_name** : (optional) , the mailbox to open on an initial connection, if non is specified, INBOX will be used.


**enableSSL** : (optional) , This is for those using a custom port, so that the library will know if the port needs a secure connection 



###Switching Mailbox 
After Connecting to the server, the optional mailbox_name  supplied in the connection parameters is opened automatically , you can change or switch mailbox using 

```php
$imap-> switchMailBox('mailbox_name');
```
the mailbox_name must be one of the names of the mailfolders or boxes in your mailserver , example: INBOX , Drafts , Spam  or Trash 

###Fetch MailBoxes (Mail Folders) 

You can also list available mail boxes in mailserver 

```php
$mailBoxes = $imap->getMailBoxes($pattern);
```

$pattern: (optional) This is either \* or % where :


\* Means the library should fetch all the mailboxes including top level mail boxes and sub folders


**%**  Means the library should fetch all sub folders in the current mailbox 


read more here : http://php.net/manual/en/function.imap-getmailboxes.php




###Get Info About a mailbox 

```php
$mailBoxInfo = $imap->getMailBoxInfo($forceNew=false);
```

**$forceNew** : (optional) The mailbox info is always prefetched and kept, but if you want a fresh copy of the info to be refetched , set this to true ..



***Count Total Recent Messages**
```php
$totalRecent = $imap->getMailBoxInfo()->recent;
```

###Count Total Unread Messages In the MailBox 

```php
$totalMsgNo = $imap->getTotalMessages();

//or

$totalMsgNo = $imap->getMailBoxInfo()->messages;
```

###Fetch Mails From a MailBox 
This library has many ways to fetch mails from a mailbox , we have also optimized this particular feature for speed and performance ..

####Fetch Mail By UID 

```php
$mails = $imap->fetchMailBoxItems("mailbox_name")
              ->select(uid1,uid2,uid3,uid4...uidN)
			  ->orderById('desc')
			  ->getResults();
```
