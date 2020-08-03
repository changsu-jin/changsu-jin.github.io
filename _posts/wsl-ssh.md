[SSH 서버 세팅하기](https://www.tuwlab.com/ece/29302)

WSL환경에서 맥으로 작업환경 옮기면서 SSH접속이 되면 편할 거 같아서 세팅한건데 맥에서 wsl로 ssh 접근 필요하신 분은 사용하시면 될 듯 합니다.

링크(https://www.tuwlab.com/ece/29302) 참조하셔서 wsl에 ssh 서버 설치하시고 첨부 한 zip파일 받으셔서 route.bat 파일 시작프로그램에 넣으시면 됩니다.

route.bat : 파일안에 파워셀스크립트 파일 경로 자신의 경로대로 변경 필요

```shell
bash.exe -c "sudo service ssh start"
powershell.exe -ExecutionPolicy Bypass -File D:\wsl2-network.ps1
```



wsl2-network.ps1 : 파일안에 $ports 변수에 포워딩할 포트 지정

```powershell
$remoteport = bash.exe -c "ifconfig eth0 | grep 'inet '"
$found = $remoteport -match '\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}';

if( $found ){
  $remoteport = $matches[0];
} else{
  echo "The Script Exited, the ip address of WSL 2 cannot be found";
  exit;
}

#[Ports]

#All the ports you want to forward separated by coma
$ports=@(80,443,10000,3000,5000,22);


#[Static ip]
#You can change the addr to your ip config to listen to a specific address
$addr='0.0.0.0';
$ports_a = $ports -join ",";


#Remove Firewall Exception Rules
iex "Remove-NetFireWallRule -DisplayName 'WSL 2 Firewall Unlock' ";

#adding Exception Rules for inbound and outbound Rules
iex "New-NetFireWallRule -DisplayName 'WSL 2 Firewall Unlock' -Direction Outbound -LocalPort $ports_a -Action Allow -Protocol TCP";
iex "New-NetFireWallRule -DisplayName 'WSL 2 Firewall Unlock' -Direction Inbound -LocalPort $ports_a -Action Allow -Protocol TCP";

for( $i = 0; $i -lt $ports.length; $i++ ){
  $port = $ports[$i];
  iex "netsh interface portproxy delete v4tov4 listenport=$port listenaddress=$addr";
  iex "netsh interface portproxy add v4tov4 listenport=$port listenaddress=$addr connectport=$port connectaddress=$remoteport";
}
```

