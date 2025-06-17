PHP için UBL-TR Schematron<br /><br />
=====================<br /><br />
E-Fatura UBL-TR Schematron Doğrulama Örneği içermektedir. http://netgenel.net - http://phpkf.com<br /><br />
Bu Java yazılımı http://kodcu.com 'un "Schematron-Validation" isimli örneğinden derlenmiştir.<br /><br />

<pre>
&lt;?php 
	/* UBL Kontrolü */
	function UBLTesti($java_dosyasi, $ubl_dosyasi, $fatura_dosyasi, $return=false)
	{
		/* Java Dosyasına Veri Yollanıyor */
		exec("java -Dfile.encoding=UTF8 -jar $java_dosyasi $ubl_dosyasi $fatura_dosyasi 2>&1", $cikti);

		/* Önemsiz Hatalar Temizleniyor */
		foreach($cikti as $anahtar => $deger)
		{
			if(stristr($deger, 'The child axis starting') == true)
				unset($cikti[$anahtar]);
			if(stristr($deger, 'Warning: on line') == true)
				unset($cikti[$anahtar]);
		}

		/* Hatalar Ekrana Yazdırılıyor */			
		if($return == false)
		{
			if(count($cikti) > 0)
			{
				foreach($cikti as $ciktiSonuc)
					echo $ciktiSonuc.'&lt;br /&gt;';
				exit();
			}
		}
		else return $cikti;
	}
	/* UBL Kontrolü */


	$dizin = (__DIR__ . DIRECTORY_SEPARATOR);
	$java_dosyasi=$dizin."Java/NTG_UBLTR_Schematron.jar";
	$ubl_dosyasi=$dizin."Schematron/UBL-TR_Main_Schematron.xml";
	$fatura_dosyasi='TemelFaturaOrnegi.xml';

	$cikti = UBLTesti($java_dosyasi, $ubl_dosyasi, $fatura_dosyasi, true);

	if(count($cikti) > 0)
	{
		foreach($cikti as $ciktiSonuc)
			echo $ciktiSonuc.'&lt;br /&gt;';
	}
	else echo 'Hata Yok...';
?&gt;
</pre>
