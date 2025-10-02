import React, { useState } from "react";
import { motion } from "framer-motion";
import {
  Wifi,
  MapPin,
  ShieldCheck,
  RefreshCcw,
  Settings,
  ArrowRight,
  Globe,
  Lock,
} from "lucide-react";

// Single-file React component showcasing a polished dark-themed VPN UI
// Tailwind CSS assumed available in the project (no import required here)
// Uses framer-motion for subtle animations and lucide-react for icons.

const REGIONS = [
  { id: "us-west", name: "United States — West", ping: 38 },
  { id: "us-east", name: "United States — East", ping: 55 },
  { id: "de", name: "Germany", ping: 82 },
  { id: "jp", name: "Japan", ping: 120 },
  { id: "sg", name: "Singapore", ping: 140 },
  { id: "afg", name: "Kabul" , ping: 200 }
];

function AnonymousLogo({ size = 56 }) {
  // Abstract anonymous/hooded logo SVG — unique and not a direct copy of any trademark.
  return (
    <svg
      width={size}
      height={size}
      viewBox="0 0 64 64"
      fill="none"
      xmlns="http://www.w3.org/2000/svg"
      aria-hidden
    >
      <circle cx="32" cy="32" r="32" fill="url(#g)" />
      <defs>
        <linearGradient id="g" x1="0" x2="1" y1="0" y2="1">
          <stop offset="0" stopColor="#0f172a" />
          <stop offset="1" stopColor="#0b1220" />
        </linearGradient>
      </defs>
      <path
        d="M12 42c0-10 9-20 20-20s20 10 20 20H12z"
        fill="#0ea5a4"
        opacity="0.12"
      />
      <path
        d="M32 14c-7 0-14 5-16 11 0 0 4-3 8-4 4-1 8-1 8-1s4 0 8 1c4 1 8 4 8 4-2-6-9-11-16-11z"
        fill="#7dd3fc"
        opacity="0.16"
      />
      <ellipse cx="26.5" cy="36.5" rx="4.5" ry="6" fill="#fff" opacity="0.95" />
      <ellipse cx="38" cy="36" rx="4" ry="5" fill="#fff" opacity="0.95" />
      <path
        d="M32 44c-4-2-8-6-10-11 4 3 10 3 10 3s6 0 10-3c-2 5-6 9-10 11z"
        fill="#000"
        opacity="0.65"
      />
    </svg>
  );
}

export default function VPNUI() {
  const [connected, setConnected] = useState(false);
  const [region, setRegion] = useState(REGIONS[0]);
  const [loadingRegion, setLoadingRegion] = useState(false);
  const [showRegions, setShowRegions] = useState(false);

  function toggleConnect() {
    // small simulated connect flow
    if (connected) {
      setConnected(false);
    } else {
      setConnected(true);
    }
  }

  function quickChange(newRegion) {
    setLoadingRegion(true);
    // simulate fast change with a tiny delay
    setTimeout(() => {
      setRegion(newRegion);
      setLoadingRegion(false);
      setConnected(true);
    }, 550);
  }

  return (
    <div className="min-h-screen flex items-center justify-center bg-gradient-to-b from-[#071122] to-[#03040a] p-6">
      <div className="w-[980px] max-w-full grid grid-cols-2 gap-6">
        {/* Left panel: main status */}
        <div className="bg-[#071226] border border-[#0b1220] rounded-2xl p-6 shadow-xl">
          <div className="flex items-center justify-between">
            <div className="flex items-center gap-3">
              <div className="w-14 h-14 flex items-center justify-center bg-gradient-to-br from-[#05202a] to-[#071226] rounded-xl shadow-inner">
                <AnonymousLogo size={48} />
              </div>
              <div>
                <div className="text-sm text-slate-400">ANON — Private VPN</div>
                <div className="text-white font-semibold text-lg">Always free • No ads • No premium</div>
              </div>
            </div>
            <div className="flex items-center gap-3">
              <button className="text-slate-400 text-sm flex items-center gap-2 px-3 py-2 rounded-md hover:bg-white/3">
                <Lock size={16} /> Encrypted
              </button>
              <button className="text-slate-400 text-sm flex items-center gap-2 px-3 py-2 rounded-md hover:bg-white/3">
                <Settings size={16} />
              </button>
            </div>
          </div>

          <div className="mt-6 grid grid-cols-1 gap-4">
            <div className="rounded-xl bg-gradient-to-b from-[#071226]/40 to-black/10 p-6 flex items-center justify-between">
              <div className="flex items-center gap-4">
                <div className="bg-[#03161a] p-4 rounded-lg">
                  <Wifi size={28} />
                </div>
                <div>
                  <div className="text-xs text-slate-400">Connection</div>
                  <div className="text-white text-2xl font-semibold">{connected ? "Connected" : "Disconnected"}</div>
                </div>
              </div>

              <motion.button
                whileTap={{ scale: 0.98 }}
                onClick={toggleConnect}
                className={`px-6 py-3 rounded-full font-semibold shadow-2xl flex items-center gap-2 transition-all ${connected ? "bg-gradient-to-r from-[#06b6d4] to-[#7c3aed] text-black" : "bg-white/5 text-white hover:bg-white/8"}`}
              >
                {connected ? (
                  <ShieldCheck size={18} />
                ) : (
                  <ArrowRight size={18} />
                )}
                {connected ? "Secure" : "Connect"}
              </motion.button>
            </div>

            <div className="rounded-xl p-4 bg-[#06141b]/40 border border-[#0b1620]">
              <div className="flex items-center justify-between">
                <div className="flex items-center gap-3">
                  <MapPin size={18} />
                  <div>
                    <div className="text-xs text-slate-400">Region</div>
                    <div className="text-white font-medium">{region.name}</div>
                  </div>
                </div>
                <div className="flex items-center gap-2">
                  <div className="text-sm text-slate-400">Ping {region.ping}ms</div>
                  <button
                    onClick={() => setShowRegions((s) => !s)}
                    className="px-3 py-1 rounded-md bg-white/3 text-white text-sm"
                  >
                    Change
                  </button>
                </div>
              </div>

              {showRegions && (
                <div className="mt-3 grid gap-2">
                  {REGIONS.map((r) => (
                    <button
                      key={r.id}
                      onClick={() => quickChange(r)}
                      className="w-full text-left px-3 py-2 rounded-md hover:bg-white/3 flex items-center justify-between"
                    >
                      <div className="flex items-center gap-3">
                        <Globe size={16} />
                        <div>
                          <div className="text-sm">{r.name}</div>
                          <div className="text-xs text-slate-400">{r.ping} ms</div>
                        </div>
                      </div>
                      {loadingRegion && r.id === region.id ? <RefreshCcw className="animate-spin" size={16} /> : null}
                    </button>
                  ))}
                </div>
              )}
            </div>

            <div className="rounded-xl p-4 bg-[#06141b]/30 border border-[#0b1620] flex items-center justify-between">
              <div>
                <div className="text-xs text-slate-400">Features</div>
                <div className="text-white font-medium">Kill switch • AES-256 • Zero logs</div>
              </div>
              <div className="text-sm text-slate-400">Free • Forever</div>
            </div>

            <div className="mt-3 text-xs text-slate-400">Quick actions</div>
            <div className="flex gap-3">
              <button onClick={() => quickChange(REGIONS[0])} className="flex-1 px-4 py-3 rounded-lg bg-white/5 text-white hover:bg-white/7">
                Fast US
              </button>
              <button onClick={() => quickChange(REGIONS[2])} className="flex-1 px-4 py-3 rounded-lg bg-white/5 text-white hover:bg-white/7">
                Fast DE
              </button>
              <button onClick={() => quickChange(REGIONS[3])} className="flex-1 px-4 py-3 rounded-lg bg-white/5 text-white hover:bg-white/7">
                Fast JP
              </button>
            </div>

            <div className="mt-4 text-[11px] text-slate-500">No premium tiers, no paywalls — just pick a region and connect. App is designed to avoid asking for subscriptions or showing upsells.</div>
          </div>
        </div>

        {/* Right panel: stats & settings */}
        <div className="bg-[#071226] border border-[#0b1220] rounded-2xl p-6 shadow-xl flex flex-col gap-4">
          <div className="flex items-center justify-between">
            <div>
              <div className="text-slate-400 text-sm">Session</div>
              <div className="text-white font-semibold">{connected ? "Active" : "Idle"}</div>
            </div>
            <div className="text-slate-400 text-sm">Data • 0.2 GB</div>
          </div>

          <div className="rounded-xl p-4 bg-[#041018]/40 flex-1">
            <div className="text-slate-400 text-sm">Live traffic (simulated)</div>
            <div className="mt-3 h-40 rounded-md bg-gradient-to-b from-black/20 to-transparent flex items-center justify-center">
              <div className="text-slate-400">Traffic graph placeholder</div>
            </div>
          </div>

          <div className="rounded-xl p-4 bg-[#041018]/30 border border-[#0b1620]">
            <div className="flex items-center justify-between">
              <div>
                <div className="text-slate-400 text-sm">Security</div>
                <div className="text-white font-medium">AES-256 • No logs</div>
              </div>
              <div className="flex items-center gap-3">
                <div className="text-xs text-slate-400">Auto-reconnect</div>
                <div className="w-12 h-6 rounded-full bg-white/6 flex items-center p-1">
                  <div className={`w-4 h-4 rounded-full bg-white ${connected ? "ml-6" : "ml-0"} transition-all`} />
                </div>
              </div>
            </div>
          </div>

          <div className="rounded-xl p-4 bg-[#041018]/10 border border-[#0b1620] flex items-center justify-between">
            <div className="text-sm text-slate-400">About</div>
            <div className="text-xs text-slate-500">v1.0 • Built for privacy</div>
          </div>

          <div className="flex gap-3 mt-2">
            <button className="flex-1 px-4 py-3 rounded-lg bg-white/5 text-white text-sm">Share</button>
            <button className="flex-1 px-4 py-3 rounded-lg bg-white/5 text-white text-sm">Feedback</button>
          </div>

          <div className="mt-2 text-[11px] text-slate-500">This mock is designed for a free VPN app — remove any analytics or telemetry when building a production privacy-first client.</div>
        </div>
      </div>
    </div>
  );
}
