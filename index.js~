const express=require("express");
const bparse=require("body-parser");
const fetch=require("node-fetch");
const app = express();
app.use(bparse.json());
app.use(bparse.urlencoded());
const cookies = "__stripe_mid=a5f3a463-8b55-4599-bbd0-e8344e6f646c; _ga=GA1.2.877846978.1578885730; __stripe_sid=ef4ae2b0-4ac5-457e-b779-879982948e50; _gid=GA1.2.310857404.1579407293; jv_enter_ts_S7sPtVuaS8=1579407293830; jv_visits_count_S7sPtVuaS8=2; _gat_gtag_UA_134239353_1=1; jv_pages_count_S7sPtVuaS8=4";
app.post("/sms/arizet", (req,res)=> {
    const msg = req.body.Body;
    fetch("https://arizet.com/httpsapi11/companies_by_prefix?prefix="+msg+"&output=essentials&count=15", {method: "GET", headers: {Cookie: cookies}}).then(resp => resp.json().then(json => {
        if (json.Status == "ok"){
            if (json.Data.length == 0){
                res.send(`<Response>
                <Message>
                We could not find that stock.
                </Message>
                </Response>`)
            }else{
                fetch("https://arizet.com/httpsapi11/technicals/v2/last_available_date", {method: "GET", headers: {Cookie: cookies}}).then(resp => resp.json().then(date => {
                    const thedate = new Date(date.Data.Data);
                    const month = thedate.getUTCMonth() + 1;
                    const day = thedate.getUTCDate();
                    const year = thedate.getUTCFullYear();
                    const timestamp=`${year}-${month}-${day}`;
                    fetch(`https://arizet.com/httpsapi11/technicals/v2/summary_stats_by_ticker?ticker=${json.Data[0].t}&date=${timestamp}`, {method: "GET", headers: {Cookie: cookies}}).then(resp => resp.json().then(stockinfo => {
                        res.send(`<Response>
                        <Message>
                        STOCK: ${stockinfo.Data._id} Info
                        </Message>
                        <Message>
                        ${stockinfo.Data.desc} -- Score: ${stockinfo.Data.score} -- Signal: ${stockinfo.Data.signal}
                        </Message>
                        </Response>`)
                    }))
                }))
            }
        }else{
            res.send(`<Response>
            <Message>
            Something went wrong while contacting stock server.
            </Message>
            </Response>`)
        }
    }))
})
app.listen(80)