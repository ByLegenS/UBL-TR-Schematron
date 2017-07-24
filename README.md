PHP için UBL-TR Schematron
=====================
Bu Java yazılımı http://kodcu.com 'un "Schematron-Validation" isimli örneğinden derlenmiştir.
E-Fatura UBL-TR Schematron Doğrulama Örneği içermektedir.
http://netgenel.net

<pre>
&lt;?php 
	function ublKontrol($java_dosyasi, $ubl_dosyasi, $fatura_dosyasi)
	{
		exec("java -Dfile.encoding=UTF8 -jar $java_dosyasi $ubl_dosyasi $fatura_dosyasi 2>&1", $cikti);
		foreach($cikti as $anahtar => $deger)
		{
			if(stristr($deger, 'The child axis starting') == true)
				unset($cikti[$anahtar]);
			if(stristr($deger, 'Warning: on line') == true)
				unset($cikti[$anahtar]);
		}
		if(count($cikti) > 0) return $cikti;
		else return false;
	}
	$dizin = (__DIR__ . DIRECTORY_SEPARATOR);
	$java_dosyasi=$dizin."Java/NTG_UBLTR_Schematron.jar";
	$ubl_dosyasi=$dizin."schematron/UBL-TR_Main_Schematron.xml";
	$fatura_dosyasi=$dizin."TemelFatura.xml";
	$cikti = ublKontrol($java_dosyasi, $ubl_dosyasi, $fatura_dosyasi);
	echo '&lt;pre&gt;';
	print_r($cikti);
	echo '&lt;/pre&gt;';
?&gt;
</pre>
