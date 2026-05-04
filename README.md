import React, { useState, useRef } from "react"; import { Card, CardContent } from "@/components/ui/card"; import { Button } from "@/components/ui/button"; import { Input } from "@/components/ui/input"; import { Download, QrCode, Upload } from "lucide-react"; import QRCode from "qrcode";

export default function QRApp() { const [text, setText] = useState(""); const [qrUrl, setQrUrl] = useState(""); const canvasRef = useRef(null);

const generateQR = async () => { if (!text) return; const url = await QRCode.toDataURL(text, { errorCorrectionLevel: "H", margin: 2, scale: 8, color: { dark: "#000000", light: "#ffffff", }, }); setQrUrl(url); };

const downloadQR = () => { const link = document.createElement("a"); link.href = qrUrl; link.download = "qrcode.png"; link.click(); };

const handleFile = (e) => { const file = e.target.files[0]; if (!file) return; const reader = new FileReader(); reader.onload = () => { setQrUrl(reader.result); }; reader.readAsDataURL(file); };

return ( <div className="min-h-screen bg-gradient-to-br from-slate-900 to-slate-800 text-white flex items-center justify-center p-6"> <Card className="w-full max-w-xl rounded-2xl shadow-xl bg-white/10 backdrop-blur"> <CardContent className="p-6 space-y-6"> <div className="flex items-center gap-2 text-xl font-semibold"> <QrCode /> Advanced QR Tool </div>

<Input
        placeholder="Enter text or URL..."
        value={text}
        onChange={(e) => setText(e.target.value)}
        className="text-black"
      />

      <div className="flex gap-3">
        <Button onClick={generateQR} className="flex-1">
          Generate
        </Button>
        <Button
          variant="secondary"
          onClick={downloadQR}
          disabled={!qrUrl}
        >
          <Download className="w-4 h-4 mr-2" /> Download
        </Button>
      </div>

      <div className="flex flex-col items-center gap-4">
        {qrUrl && (
          <img
            src={qrUrl}
            alt="QR Code"
            className="rounded-xl shadow-lg"
          />
        )}
      </div>

      <div className="border-t border-white/20 pt-4">
        <label className="flex items-center gap-2 cursor-pointer">
          <Upload /> Upload QR Image
          <input type="file" hidden onChange={handleFile} />
        </label>
      </div>
    </CardContent>
  </Card>
</div>

); }# qr-code-app