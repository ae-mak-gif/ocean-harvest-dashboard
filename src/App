import { FormEvent, ReactNode, useMemo, useState } from "react";

type PageKey =
  | "overview" | "procurement" | "inventory" | "transfers" | "sales"
  | "cashbook" | "assets" | "team" | "evidence" | "reports" | "settings";
type Row = Record<string, string | number | ReactNode>;

const money = (value: number, currency = "USD") =>
  new Intl.NumberFormat("en-ZW", { style: "currency", currency, maximumFractionDigits: 0 }).format(value);
const num = (value: number) => new Intl.NumberFormat("en-ZW").format(value);

const nav: { key: PageKey; label: string; icon: string }[] = [
  { key: "overview", label: "Owner overview", icon: "grid" },
  { key: "procurement", label: "Procurement", icon: "basket" },
  { key: "inventory", label: "Inventory & weighing", icon: "scale" },
  { key: "transfers", label: "Transfers", icon: "truck" },
  { key: "sales", label: "Sales & retailers", icon: "receipt" },
  { key: "cashbook", label: "Cashbook", icon: "wallet" },
  { key: "assets", label: "Boat & assets", icon: "anchor" },
  { key: "team", label: "Employees & access", icon: "users" },
  { key: "evidence", label: "Evidence centre", icon: "image" },
  { key: "reports", label: "Reports & insights", icon: "chart" },
  { key: "settings", label: "Settings", icon: "settings" },
];

const purchases = [
  { ref: "PUR-0842", supplier: "Tawanda Moyo", buyer: "Rudo Ncube", species: "Bream", kg: 1240, rate: 2.45, total: 3038, date: "18 Jun", evidence: "Complete", advance: "ADV-220" },
  { ref: "PUR-0841", supplier: "Nyaminyami Co-op", buyer: "Farai Sibanda", species: "Kapenta", kg: 980, rate: 1.85, total: 1813, date: "17 Jun", evidence: "Missing scale photo", advance: "ADV-219" },
  { ref: "PUR-0839", supplier: "M. Chuma", buyer: "Rudo Ncube", species: "Tiger fish", kg: 610, rate: 2.75, total: 1678, date: "16 Jun", evidence: "Complete", advance: "ADV-217" },
  { ref: "PUR-0836", supplier: "Kariba Fishermen Group", buyer: "Tendai Zhou", species: "Bream", kg: 1920, rate: 2.35, total: 4512, date: "14 Jun", evidence: "Complete", advance: "ADV-214" },
];

const advances = [
  { ref: "ADV-220", employee: "Rudo Ncube", issued: 3500, purchases: 3038, expenses: 110, returned: 0, outstanding: 352, status: "Reconcile" },
  { ref: "ADV-219", employee: "Farai Sibanda", issued: 2000, purchases: 1813, expenses: 87, returned: 100, outstanding: 0, status: "Reconciled" },
  { ref: "ADV-217", employee: "Rudo Ncube", issued: 1800, purchases: 1678, expenses: 72, returned: 50, outstanding: 0, status: "Reconciled" },
  { ref: "ADV-214", employee: "Tendai Zhou", issued: 4700, purchases: 4512, expenses: 140, returned: 48, outstanding: 0, status: "Reconciled" },
];

const initialBatches = [
  { batch: "BAT-318", species: "Bream", form: "Whole · iced", purchased: 1240, available: 460, location: "Kariba", condition: "Good", cost: 3038 },
  { batch: "BAT-317", species: "Kapenta", form: "Form unconfirmed", purchased: 980, available: 380, location: "Kariba", condition: "Good", cost: 1813 },
  { batch: "BAT-315", species: "Tiger fish", form: "Whole · iced", purchased: 610, available: 210, location: "Harare", condition: "Good", cost: 1678 },
  { batch: "BAT-312", species: "Bream", form: "Whole · iced", purchased: 1920, available: 690, location: "Harare", condition: "Good", cost: 4512 },
  { batch: "BAT-311", species: "Kapenta", form: "Form unconfirmed", purchased: 440, available: 320, location: "In transit", condition: "Under review", cost: 820 },
];

const transfers = [
  { ref: "TRF-109", route: "Kariba → Harare", transporter: "Mapope Logistics", vehicle: "AEV 2847", dispatched: 760, received: 748, variance: -12, status: "Review variance", date: "18 Jun · 05:40" },
  { ref: "TRF-108", route: "Kariba → Harare", transporter: "Chiedza Carriers", vehicle: "AFD 6103", dispatched: 620, received: 620, variance: 0, status: "Received", date: "17 Jun · 06:15" },
  { ref: "TRF-107", route: "Kariba → Harare", transporter: "Mapope Logistics", vehicle: "AEV 2847", dispatched: 440, received: 0, variance: 0, status: "Awaiting receipt", date: "18 Jun · 09:10" },
];

const initialSales = [
  { invoice: "INV-1458", retailer: "Mbare Fresh Foods", location: "Harare", species: "Bream", kg: 420, rate: 4.4, total: 1848, paid: 1200, status: "Part paid", date: "18 Jun" },
  { invoice: "INV-1457", retailer: "Lakeview Grocers", location: "Kariba", species: "Kapenta", kg: 180, rate: 3.25, total: 585, paid: 585, status: "Paid", date: "18 Jun" },
  { invoice: "INV-1455", retailer: "Samora Wholesale", location: "Harare", species: "Tiger fish", kg: 260, rate: 5.1, total: 1326, paid: 0, status: "Credit", date: "17 Jun" },
  { invoice: "INV-1452", retailer: "Chinhoyi Street Market", location: "Kariba", species: "Bream", kg: 310, rate: 4.2, total: 1302, paid: 1302, status: "Paid", date: "17 Jun" },
];

const cashLocations = [
  { location: "Kariba", opening: 820, cash: 1887, bank: 0, mobile: 420, paid: 1370, expected: 1337, actual: 1337, remitted: 700, reference: "RMT-082 · Pending" },
  { location: "Harare", opening: 1250, cash: 1200, bank: 810, mobile: 385, paid: 460, expected: 2835, actual: 2823, remitted: 1200, reference: "RMT-081 · Reviewed" },
];

const evidence = [
  { id: "EVD-441", category: "Weighing", title: "BAT-318 on platform scale", related: "PUR-0842", by: "Rudo Ncube", status: "Reviewed", tone: "cyan" },
  { id: "EVD-440", category: "Loading", title: "TRF-109 cooler loading", related: "TRF-109", by: "Tendai Zhou", status: "To review", tone: "navy" },
  { id: "EVD-438", category: "Delivery", title: "Harare receipt condition", related: "TRF-109", by: "Nyasha Dube", status: "Flagged", tone: "blue" },
  { id: "EVD-435", category: "Fuel receipt", title: "Kariba marina fuel receipt", related: "FUEL-077", by: "Blessing Maki", status: "Reviewed", tone: "teal" },
  { id: "EVD-431", category: "Maintenance", title: "Port-side hull inspection", related: "MNT-024", by: "Tinashe Marine", status: "To review", tone: "slate" },
  { id: "EVD-429", category: "Purchase receipt", title: "Nyaminyami Co-op receipt", related: "PUR-0841", by: "Farai Sibanda", status: "Reviewed", tone: "ice" },
];

const retailers = [
  { name: "Mbare Fresh Foods", location: "Mbare, Harare", contact: "L. Muswere", balance: 648, terms: "7 days" },
  { name: "Samora Wholesale", location: "Harare CBD", contact: "P. Dzinotyiwei", balance: 1326, terms: "14 days" },
  { name: "Lakeview Grocers", location: "Kariba Heights", contact: "S. Tembo", balance: 0, terms: "Cash" },
];

const employees = [
  { name: "Chipo Mhlanga", role: "Owner / Admin", location: "Both locations", responsibility: "Full oversight, approvals and reports", status: "Active" },
  { name: "Rudo Ncube", role: "Kariba Buying Team", location: "Kariba", responsibility: "Advances, purchasing and weighing", status: "Active" },
  { name: "Farai Sibanda", role: "Kariba Buying Team", location: "Kariba", responsibility: "Supplier records and evidence", status: "Active" },
  { name: "Nyasha Dube", role: "Harare Sales Team", location: "Harare", responsibility: "Receiving, sales and collections", status: "Active" },
  { name: "Kudakwashe Moyo", role: "Harare Sales Team", location: "Harare", responsibility: "Retailers and daily cash-up", status: "Active" },
];

function Icon({ name, size = 18 }: { name: string; size?: number }) {
  const paths: Record<string, ReactNode> = {
    grid: <><rect x="3" y="3" width="7" height="7" rx="2"/><rect x="14" y="3" width="7" height="7" rx="2"/><rect x="3" y="14" width="7" height="7" rx="2"/><rect x="14" y="14" width="7" height="7" rx="2"/></>,
    basket: <><path d="M4 10h16l-2 10H6L4 10Z"/><path d="m8 10 4-7 4 7M9 14v2m6-2v2"/></>,
    scale: <><path d="M12 3v18M5 6h14M4 6l-3 7h6L4 6Zm16 0-3 7h6l-3-7Z"/><path d="M8 21h8"/></>,
    truck: <><path d="M3 6h11v11H3zM14 10h4l3 3v4h-7z"/><circle cx="7" cy="19" r="2"/><circle cx="18" cy="19" r="2"/></>,
    receipt: <><path d="M6 3h12v18l-3-2-3 2-3-2-3 2V3Z"/><path d="M9 8h6m-6 4h6"/></>,
    wallet: <><path d="M3 6h16a2 2 0 0 1 2 2v11H3V6Z"/><path d="M3 6V4h14v2m1 6h3"/></>,
    anchor: <><circle cx="12" cy="5" r="2"/><path d="M12 7v14M5 12H2c0 5 4 9 10 9s10-4 10-9h-3M8 11h8"/></>,
    users: <><circle cx="9" cy="8" r="4"/><path d="M2 21v-2c0-4 3-7 7-7s7 3 7 7v2m0-9c3 0 6 3 6 7v2"/></>,
    image: <><rect x="3" y="4" width="18" height="16" rx="2"/><circle cx="9" cy="10" r="2"/><path d="m3 17 5-4 4 3 3-2 6 4"/></>,
    chart: <><path d="M4 20V10m6 10V4m6 16v-7m5 7H2"/></>,
    settings: <><circle cx="12" cy="12" r="3"/><path d="M19 12a7 7 0 0 0-.1-1l2-1.5-2-3.4-2.4 1A8 8 0 0 0 15 6l-.3-3h-4l-.3 3a8 8 0 0 0-1.5.9l-2.4-1-2 3.4 2 1.6a7 7 0 0 0 0 2.2l-2 1.6 2 3.4 2.4-1A8 8 0 0 0 10.4 18l.3 3h4l.3-3a8 8 0 0 0 1.5-.9l2.4 1 2-3.4-2-1.6c.1-.4.1-.7.1-1.1Z"/></>,
    search: <><circle cx="11" cy="11" r="7"/><path d="m20 20-4-4"/></>,
    bell: <><path d="M18 8a6 6 0 0 0-12 0c0 7-3 7-3 9h18c0-2-3-2-3-9Z"/><path d="M10 21h4"/></>,
    plus: <path d="M12 5v14M5 12h14"/>, arrow: <path d="m9 18 6-6-6-6"/>,
    menu: <path d="M4 7h16M4 12h16M4 17h16"/>, x: <path d="m6 6 12 12M18 6 6 18"/>,
    check: <path d="m5 12 4 4L19 6"/>, dots: <><circle cx="5" cy="12" r="1"/><circle cx="12" cy="12" r="1"/><circle cx="19" cy="12" r="1"/></>,
    upload: <><path d="M12 16V3m-5 5 5-5 5 5"/><path d="M4 15v5h16v-5"/></>,
  };
  return <svg width={size} height={size} viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="1.8" strokeLinecap="round" strokeLinejoin="round" aria-hidden>{paths[name] || paths.grid}</svg>;
}

function Button({ children, onClick, variant = "primary", type = "button" }: { children: ReactNode; onClick?: () => void; variant?: string; type?: "button" | "submit" }) {
  return <button type={type} className={`btn btn-${variant}`} onClick={onClick}>{children}</button>;
}
function Status({ children }: { children: ReactNode }) {
  const t = String(children).toLowerCase();
  const kind = t.includes("paid") || t.includes("complete") || t.includes("reviewed") || t.includes("active") || t.includes("received") || t.includes("reconciled") ? "good" : t.includes("missing") || t.includes("flag") || t.includes("variance") || t.includes("due") ? "bad" : "warn";
  return <span className={`status ${kind}`}><i />{children}</span>;
}
function SectionTitle({ title, text, action }: { title: string; text?: string; action?: ReactNode }) {
  return <div className="section-head"><div><div className="section-title">{title}</div>{text && <div className="section-text">{text}</div>}</div>{action}</div>;
}
function Card({ children, className = "" }: { children: ReactNode; className?: string }) {
  return <section className={`card ${className}`}>{children}</section>;
}
function DataTable({ columns, rows, onRow }: { columns: { key: string; label: string }[]; rows: Row[]; onRow?: (r: Row) => void }) {
  return <div className="table-wrap"><table><thead><tr>{columns.map(c => <th key={c.key}>{c.label}</th>)}<th /></tr></thead><tbody>{rows.map((r, i) => <tr key={i} onClick={() => onRow?.(r)} className={onRow ? "clickable" : ""}>{columns.map(c => <td key={c.key}>{r[c.key]}</td>)}<td><Icon name="arrow" size={15}/></td></tr>)}</tbody></table></div>;
}
function PageIntro({ eyebrow, title, text, children }: { eyebrow: string; title: string; text: string; children?: ReactNode }) {
  return <div className="page-intro"><div><div className="eyebrow">{eyebrow} · DEMO DATA</div><div className="page-title">{title}</div><div className="page-desc">{text}</div></div><div className="intro-actions">{children}</div></div>;
}

function Modal({ title, children, onClose }: { title: string; children: ReactNode; onClose: () => void }) {
  return <div className="modal-backdrop" onMouseDown={onClose}><div className="modal" onMouseDown={e => e.stopPropagation()}><div className="modal-head"><div><div className="eyebrow">BROWSER-LOCAL DEMO</div><div className="modal-title">{title}</div></div><Button variant="icon" onClick={onClose}><Icon name="x"/></Button></div>{children}</div></div>;
}

function SimpleForm({ type, onClose, onSave }: { type: string; onClose: () => void; onSave: (type: string) => void }) {
  const fields: Record<string, string[]> = {
    "Record purchase": ["Supplier", "Species", "Net weight (kg)", "Price per kg", "Advance reference"],
    "Issue advance": ["Employee", "Amount", "Purpose", "Issue date", "Approval reference"],
    "Record weighing": ["Batch reference", "Gross weight (kg)", "Tare weight (kg)", "Condition", "Weighing employee"],
    "Record dispatch": ["Transfer reference", "Transporter", "Vehicle identifier", "Dispatched weight (kg)", "Batch references"],
    "Record receipt": ["Transfer reference", "Received weight (kg)", "Condition on arrival", "Receiving employee", "Arrival notes"],
    "Add sale": ["Retailer", "Species", "Weight (kg)", "Price per kg", "Payment type"],
    "Record payment": ["Invoice reference", "Amount", "Payment method", "Receipt reference", "Received by"],
    "Record fuel": ["Trip reference", "Quantity (litres)", "Cost", "Supplier", "Receipt reference"],
    "Report damage": ["Asset", "Damage description", "Estimated repair cost", "Reported by", "Date"],
    "Upload evidence": ["Category", "Related transaction", "Description", "Submitted by", "Review status"],
  };
  const inputs = fields[type] || ["Reference", "Amount", "Date", "Notes"];
  const submit = (e: FormEvent) => { e.preventDefault(); onSave(type); };
  return <Modal title={type} onClose={onClose}><form onSubmit={submit} className="form-grid">{inputs.map((f, i) => <label key={f} className={i === inputs.length - 1 ? "wide" : ""}><span>{f}</span>{f.includes("Species") || f.includes("status") || f.includes("Status") || f.includes("method") || f.includes("type") || f.includes("Condition") ? <select defaultValue=""><option value="" disabled>Select option</option><option>Bream</option><option>Kapenta</option><option>Tiger fish</option><option>Cash</option><option>Good</option></select> : <input required placeholder={f.includes("Date") ? "18 Jun 2025" : `Enter ${f.toLowerCase()}`} />}</label>)}<div className="upload-zone wide"><Icon name="upload"/><div><b>Add supporting evidence</b><span>Optional local preview · JPG or PNG</span></div></div><div className="form-actions wide"><Button variant="secondary" onClick={onClose}>Cancel</Button><Button type="submit"><Icon name="check"/> Save demo record</Button></div></form></Modal>;
}

function MiniBars({ values, labels, color = "blue" }: { values: number[]; labels: string[]; color?: string }) {
  const max = Math.max(...values);
  return <div className="mini-chart"><div className="bars">{values.map((v, i) => <div className="bar-col" key={i}><div className={`bar ${color}`} style={{ height: `${Math.max(10, v / max * 100)}%` }}><span>{v}</span></div></div>)}</div><div className="bar-labels">{labels.map(x => <span key={x}>{x}</span>)}</div></div>;
}

function Overview({ currency, open, go }: { currency: string; open: (x: string) => void; go: (x: PageKey) => void }) {
  const totalKg = purchases.reduce((a, b) => a + b.kg, 0);
  const stock = initialBatches.reduce((a, b) => a + b.available, 0);
  const salesTotal = initialSales.reduce((a, b) => a + b.total, 0);
  const soldKg = initialSales.reduce((a, b) => a + b.kg, 0);
  const outstanding = advances.reduce((a, b) => a + b.outstanding, 0);
  const credit = initialSales.reduce((a, b) => a + b.total - b.paid, 0);
  const cards = [
    ["Today's sales", money(2433, currency), "Both locations · invoiced", "+8.4%"],
    ["Fish purchased", `${num(totalKg)} kg`, "This operating week", "79% of 6t"],
    ["Fish sold", `${num(soldKg)} kg`, "Across 4 invoices", "1,170 kg"],
    ["Available stock", `${num(stock)} kg`, "Excludes in-transit", "2 locations"],
    ["Buying advances", money(outstanding, currency), "1 needs reconciliation", "Review"],
    ["Pending remittances", money(700, currency), "Kariba · RMT-082", "Pending"],
    ["Retailer credit", money(credit, currency), "2 open balances", "Due"],
    ["Est. gross margin", money(salesTotal - 3002, currency), "Revenue less est. COGS", "33.5%"],
  ];
  return <><PageIntro eyebrow="Owner control room" title="Good morning, Chipo" text="A live operational picture from lake procurement to retailer collections."><Button variant="secondary" onClick={() => open("Review daily position")}>Review cash-up</Button><Button onClick={() => open("Record purchase")}><Icon name="plus"/> New purchase</Button></PageIntro>
    <div className="metric-grid">{cards.map((c, i) => <Card className="metric" key={c[0]}><div className="metric-top"><span>{c[0]}</span><i className={`metric-icon m${i}`}><Icon name={nav[(i + 1) % nav.length].icon}/></i></div><strong>{c[1]}</strong><div className="metric-foot"><span>{c[2]}</span><b>{c[3]}</b></div></Card>)}</div>
    <div className="overview-grid">
      <Card className="span-2"><SectionTitle title="Weekly procurement" text="Purchases versus configured 4,000–6,000 kg target" action={<span className="legend"><i/>4,750 kg actual</span>}/><MiniBars values={[540, 620, 450, 760, 980, 790, 610]} labels={["Wed","Thu","Fri","Sat","Sun","Mon","Tue"]}/><div className="target-line"><span>Minimum target</span><b>{num(totalKg)} / 6,000 kg</b><div><i style={{ width: `${totalKg / 60}%` }}/></div></div></Card>
      <Card><SectionTitle title="Needs attention" text="6 items across operations"/><div className="attention-list">
        {[["ADV-220","Advance has $352 outstanding","procurement"],["PUR-0841","Missing weighing evidence","procurement"],["TRF-109","12 kg receipt variance","transfers"],["RMT-082","Remittance awaits review","cashbook"],["MNT-024","Boat hull inspection due","assets"]].map(x => <button key={x[0]} onClick={() => go(x[2] as PageKey)}><span><b>{x[0]}</b>{x[1]}</span><Icon name="arrow"/></button>)}
      </div></Card>
      <Card><SectionTitle title="Sales by location" text="Invoiced revenue · this week"/><div className="donut-row"><div className="donut"><div><b>61%</b><span>Harare</span></div></div><div className="donut-key"><span><i className="key-blue"/>Harare <b>{money(3950,currency)}</b></span><span><i className="key-cyan"/>Kariba <b>{money(2521,currency)}</b></span></div></div></Card>
      <Card><SectionTitle title="Stock by species" text="Available and in-transit kilograms"/><div className="stock-bars">{[["Bream",1150,46],["Kapenta",700,28],["Tiger fish",210,9]].map(x => <div key={x[0]}><span>{x[0]} <b>{num(x[1] as number)} kg</b></span><i><em style={{width:`${x[2]}%`}}/></i></div>)}</div></Card>
      <Card><SectionTitle title="Collections vs remittances" text="Cash movement · last 7 days"/><MiniBars values={[42,55,63,51,76,64,82]} labels={["W","T","F","S","S","M","T"]} color="teal"/><div className="chart-note"><span>Collections {money(5472,currency)}</span><span>Remitted {money(4090,currency)}</span></div></Card>
    </div></>;
}

function Procurement({ currency, open, detail }: any) {
  const rows: Row[] = purchases.map(p => ({...p, kg: `${num(p.kg)} kg`, rate: `${money(p.rate,currency)}/kg`, total: money(p.total,currency), evidence: <Status>{p.evidence}</Status>}));
  const advanceRows: Row[] = advances.map(a => ({...a, issued: money(a.issued,currency), purchases: money(a.purchases,currency), outstanding: money(a.outstanding,currency), status: <Status>{a.status}</Status>}));
  return <><PageIntro eyebrow="Kariba operations" title="Procurement & cash advances" text="Control employee buying funds, supplier purchases and end-to-end reconciliation."><Button variant="secondary" onClick={() => open("Issue advance")}>Issue advance</Button><Button onClick={() => open("Record purchase")}><Icon name="plus"/> Record purchase</Button></PageIntro>
    <div className="summary-strip"><span>Advanced this week <b>{money(12000,currency)}</b></span><span>Purchases applied <b>{money(11041,currency)}</b></span><span>Approved expenses <b>{money(409,currency)}</b></span><span>Outstanding <b>{money(352,currency)}</b></span></div>
    <Card><SectionTitle title="Buying advances" text="Outstanding = issued − purchases − approved expenses − cash returned" action={<Button variant="ghost" onClick={() => open("Record expense")}>Record expense / return</Button>}/><DataTable columns={[{key:"ref",label:"Reference"},{key:"employee",label:"Employee"},{key:"issued",label:"Issued"},{key:"purchases",label:"Purchases"},{key:"outstanding",label:"Outstanding"},{key:"status",label:"Status"}]} rows={advanceRows} onRow={detail}/></Card>
    <Card><SectionTitle title="Fish purchases" text="Fictional supplier transactions · all sample records"/><DataTable columns={[{key:"ref",label:"Purchase"},{key:"supplier",label:"Supplier / fisherman"},{key:"species",label:"Species"},{key:"kg",label:"Net weight"},{key:"rate",label:"Price"},{key:"total",label:"Total cost"},{key:"evidence",label:"Evidence"}]} rows={rows} onRow={detail}/></Card></>;
}

function Inventory({ currency, open, detail }: any) {
  const rows: Row[] = initialBatches.map(b => ({...b, purchased:`${num(b.purchased)} kg`,available:`${num(b.available)} kg`,cost:money(b.cost,currency), condition:<Status>{b.condition}</Status>}));
  return <><PageIntro eyebrow="Stock control" title="Fish inventory & weighing" text="Batch-level custody across Kariba, in transit and Harare."><Button onClick={() => open("Record weighing")}><Icon name="scale"/> Record weighing</Button></PageIntro>
    <div className="location-cards">{[["Kariba cold room","840 kg","2 active batches"],["In transit","320 kg","TRF-107"],["Harare cold room","900 kg","2 active batches"],["Sold this week","1,170 kg","4 invoices"]].map((x,i)=><Card key={x[0]}><i className={`location-icon l${i}`}><Icon name={i===1?"truck":i===3?"receipt":"basket"}/></i><span>{x[0]}</span><b>{x[1]}</b><small>{x[2]}</small></Card>)}</div>
    <Card><SectionTitle title="Batch register" text="Purchased stock less sold quantities and approved adjustments"/><DataTable columns={[{key:"batch",label:"Batch"},{key:"species",label:"Species"},{key:"form",label:"Product form"},{key:"purchased",label:"Purchased"},{key:"available",label:"Available"},{key:"location",label:"Custody"},{key:"condition",label:"Condition"}]} rows={rows} onRow={detail}/></Card>
    <div className="two-col"><Card><SectionTitle title="Latest weighing evidence" text="Illustrative preview · not authenticity verification"/><div className="evidence-preview"><div className="scale-graphic"><Icon name="scale" size={46}/><span>1,276.4</span><small>GROSS KG</small></div><div><Status>Reviewed</Status><b>BAT-318 · Bream</b><p>Gross 1,276.4 kg − tare 36.4 kg = <strong>1,240 kg net</strong></p><small>Recorded by Rudo Ncube · 18 Jun, 07:42</small></div></div></Card><Card><SectionTitle title="Stock adjustments" text="Write-offs require recorded approval"/><div className="empty-compact"><i><Icon name="check"/></i><b>No unapproved adjustments</b><span>18 kg of spoilage approved this week under ADJ-031.</span></div></Card></div></>;
}

function Transfers({ open, detail }: any) {
  const rows: Row[] = transfers.map(t => ({...t, dispatched:`${t.dispatched} kg`,received:t.received?`${t.received} kg`:"—",variance:t.received?`${t.variance} kg`:"Pending",status:<Status>{t.status}</Status>}));
  return <><PageIntro eyebrow="Chain of custody" title="Transfers & deliveries" text="Document handover from Kariba dispatch to Harare receipt without automatic blame or stock deductions."><Button variant="secondary" onClick={() => open("Record receipt")}>Record receipt</Button><Button onClick={() => open("Record dispatch")}><Icon name="plus"/> Record dispatch</Button></PageIntro>
    <Card><SectionTitle title="Active transfer lane" text="Kariba → Harare · third-party road transport"/><div className="route"><div><i><Icon name="basket"/></i><span><b>Kariba</b>Dispatch & seal</span></div><em><span>≈ 365 km</span></em><div><i><Icon name="truck"/></i><span><b>Harare</b>Receive & verify</span></div></div></Card>
    <Card><SectionTitle title="Transfer register" text="Weight differences are flagged for human review only"/><DataTable columns={[{key:"ref",label:"Transfer"},{key:"transporter",label:"Transporter"},{key:"vehicle",label:"Vehicle"},{key:"dispatched",label:"Dispatched"},{key:"received",label:"Received"},{key:"variance",label:"Variance"},{key:"status",label:"Status"}]} rows={rows} onRow={detail}/></Card></>;
}

function Sales({ currency, open, detail }: any) {
  const rows: Row[] = initialSales.map(s => ({...s,kg:`${s.kg} kg`,rate:`${money(s.rate,currency)}/kg`,total:money(s.total,currency),paid:money(s.paid,currency),balance:money(s.total-s.paid,currency),status:<Status>{s.status}</Status>}));
  return <><PageIntro eyebrow="Trade desk" title="Sales & retailers" text="Separate invoiced revenue, cash collections and retailer credit balances."><Button variant="secondary" onClick={() => open("Record payment")}>Record payment</Button><Button onClick={() => open("Add sale")}><Icon name="plus"/> Add sale</Button></PageIntro>
    <div className="summary-strip"><span>Invoiced revenue <b>{money(5061,currency)}</b></span><span>Cash collected <b>{money(3087,currency)}</b></span><span>Credit outstanding <b>{money(1974,currency)}</b></span><span>Average price <b>{money(4.33,currency)}/kg</b></span></div>
    <Card><SectionTitle title="Sales register" text="Sold quantities reduce available batch stock"/><DataTable columns={[{key:"invoice",label:"Invoice"},{key:"retailer",label:"Retailer"},{key:"location",label:"Location"},{key:"species",label:"Species"},{key:"kg",label:"Weight"},{key:"total",label:"Revenue"},{key:"paid",label:"Collected"},{key:"balance",label:"Balance"}]} rows={rows} onRow={detail}/></Card>
    <Card><SectionTitle title="Retailer directory" text="Fictional demonstration contacts"/><div className="retailer-grid">{retailers.map(r=><button key={r.name} onClick={()=>detail(r)}><i>{r.name.split(" ").map(x=>x[0]).slice(0,2).join("")}</i><span><b>{r.name}</b><small>{r.location} · {r.contact}</small></span><em><small>Balance</small><b>{money(r.balance,currency)}</b></em></button>)}</div></Card></>;
}

function Cashbook({ currency, open, detail }: any) {
  const [tab,setTab]=useState("Combined");
  const filtered = tab==="Combined"?cashLocations:cashLocations.filter(x=>x.location===tab);
  const rows:Row[]=filtered.map(x=>({...x,opening:money(x.opening,currency),cash:money(x.cash,currency),bank:money(x.bank,currency),mobile:money(x.mobile,currency),paid:money(x.paid,currency),expected:money(x.expected,currency),actual:money(x.actual,currency),variance:money(x.actual-x.expected,currency),reference:<Status>{x.reference}</Status>}));
  return <><PageIntro eyebrow="Daily control" title="Cashbook & reconciliation" text="Cash, bank and mobile money stay distinct; credit invoices do not count as receipts."><Button onClick={()=>open("Review daily position")}><Icon name="check"/> Close cash-up</Button></PageIntro>
    <div className="tabs">{["Combined","Kariba","Harare"].map(x=><button className={tab===x?"active":""} onClick={()=>setTab(x)} key={x}>{x}</button>)}</div>
    <div className="cash-metrics"><Card><span>Physical cash</span><b>{money(4160,currency)}</b><small>Before reviewed remittances</small></Card><Card><span>Bank receipts</span><b>{money(810,currency)}</b><small>Direct customer receipts</small></Card><Card><span>Mobile money</span><b>{money(805,currency)}</b><small>Both locations</small></Card><Card><span>Pending remittance</span><b>{money(700,currency)}</b><small>RMT-082 · Kariba</small></Card></div>
    <Card><SectionTitle title="Today's location reconciliation" text="Expected closing = opening cash + cash receipts − cash payments"/><DataTable columns={[{key:"location",label:"Location"},{key:"opening",label:"Opening"},{key:"cash",label:"Cash receipts"},{key:"paid",label:"Cash paid"},{key:"expected",label:"Expected"},{key:"actual",label:"Counted"},{key:"variance",label:"Variance"},{key:"reference",label:"Remittance"}]} rows={rows} onRow={detail}/></Card>
    <Card><SectionTitle title="Recent cashbook movement" text="No double-counting of owner remittances"/><DataTable columns={[{key:"ref",label:"Reference"},{key:"type",label:"Movement"},{key:"channel",label:"Channel"},{key:"location",label:"Location"},{key:"amount",label:"Amount"}]} rows={[
      {ref:"CBK-781",type:"Customer payment · INV-1458",channel:"Cash",location:"Harare",amount:money(1200,currency)},
      {ref:"CBK-780",type:"Buying advance · ADV-220",channel:"Cash",location:"Kariba",amount:`−${money(3500,currency)}`},
      {ref:"CBK-779",type:"Retail sale · INV-1457",channel:"Mobile money",location:"Kariba",amount:money(585,currency)},
      {ref:"RMT-082",type:"Owner remittance",channel:"Cash transfer",location:"Kariba",amount:`−${money(700,currency)}`},
    ]}/></Card></>;
}

function Assets({currency,open,detail}:any){
  return <><PageIntro eyebrow="Marine operations" title="Boat, fuel & maintenance" text="Owned marine assets and third-party road transport are tracked separately."><Button variant="secondary" onClick={()=>open("Report damage")}>Report damage</Button><Button onClick={()=>open("Record fuel")}><Icon name="plus"/> Record fuel</Button></PageIntro>
    <Card className="boat-card"><div className="boat-mark"><Icon name="anchor" size={32}/></div><div><div className="eyebrow">OWNED ASSET · BOAT-001</div><div className="section-title">MV Nyaminyami</div><p>Aluminium fishing boat · Kariba berth 4 · 60HP outboard</p></div><div className="asset-stats"><span><small>Condition</small><Status>Service due</Status></span><span><small>Next maintenance</small><b>21 Jun 2025</b></span><span><small>Engine hours</small><b>1,284 h</b></span></div></Card>
    <div className="two-col"><Card><SectionTitle title="Trip & fuel register" text="Operational use and fuel cost"/><DataTable columns={[{key:"ref",label:"Trip"},{key:"crew",label:"Crew"},{key:"fuel",label:"Fuel"},{key:"cost",label:"Cost"},{key:"status",label:"Condition"}]} rows={[
      {ref:"TRIP-188 · 18 Jun",crew:"B. Maki + 2",fuel:"68 L",cost:money(109,currency),status:<Status>Complete</Status>},
      {ref:"TRIP-187 · 16 Jun",crew:"B. Maki + 3",fuel:"74 L",cost:money(118,currency),status:<Status>Complete</Status>},
      {ref:"TRIP-185 · 13 Jun",crew:"T. Zhou + 2",fuel:"65 L",cost:money(104,currency),status:<Status>Damage noted</Status>},
    ]} onRow={detail}/></Card><Card><SectionTitle title="Maintenance schedule" text="Repairs and supporting evidence"/><div className="maintenance-list">{[["MNT-024","Hull & propeller inspection","Due 21 Jun","Due"],["MNT-023","Engine oil and filter","Completed 02 Jun","Complete"],["DMG-014","Port rail dent","Estimate $180","Scheduled"]].map(x=><button key={x[0]} onClick={()=>detail({reference:x[0],item:x[1],schedule:x[2]})}><i><Icon name="settings"/></i><span><b>{x[1]}</b><small>{x[0]} · {x[2]}</small></span><Status>{x[3]}</Status></button>)}</div></Card></div>
    <Card><SectionTitle title="Third-party transporter register" text="Road vehicles are not represented as company-owned assets"/><DataTable columns={[{key:"name",label:"Transporter"},{key:"contact",label:"Contact"},{key:"vehicle",label:"Vehicle used"},{key:"trips",label:"Completed trips"},{key:"status",label:"Standing"}]} rows={[{name:"Mapope Logistics",contact:"E. Zulu · 077 000 1421",vehicle:"AEV 2847",trips:18,status:<Status>Active</Status>},{name:"Chiedza Carriers",contact:"M. Chirenje · 078 000 6532",vehicle:"AFD 6103",trips:11,status:<Status>Active</Status>}]}/></Card></>;
}

function Team({role,setRole,detail}:any){
  return <><PageIntro eyebrow="People & controls" title="Employees & access" text="Responsibilities and role-based demonstration views. This is not secure authentication."/>
    <Card className="role-card"><div><span className="eyebrow">VISUAL SIMULATION ONLY</span><div className="section-title">Preview a team role</div><p>Switching roles changes emphasis only; it does not enforce access control.</p></div><div className="role-switch">{["Owner / Admin","Kariba Buying Team","Harare Sales Team"].map(x=><button key={x} onClick={()=>setRole(x)} className={role===x?"active":""}>{x}</button>)}</div></Card>
    <div className="responsibilities"><Card><i><Icon name="grid"/></i><b>Owner / Admin</b><span>Company-wide metrics, settings, approvals and all financial reports.</span></Card><Card><i><Icon name="basket"/></i><b>Kariba Buying Team</b><span>Advances, supplier purchasing, weighing, dispatch and boat records.</span></Card><Card><i><Icon name="receipt"/></i><b>Harare Sales Team</b><span>Transfer receipt, stock condition, retailer sales and collections.</span></Card></div>
    <Card><SectionTitle title="Employee directory" text="Fictional demonstration profiles"/><DataTable columns={[{key:"name",label:"Employee"},{key:"role",label:"Demo role"},{key:"location",label:"Location"},{key:"responsibility",label:"Primary responsibility"},{key:"status",label:"Status"}]} rows={employees.map(x=>({...x,status:<Status>{x.status}</Status>}))} onRow={detail}/></Card></>;
}

function Evidence({open,detail}:any){
  const [filter,setFilter]=useState("All");
  const items=filter==="All"?evidence:evidence.filter(x=>x.category===filter);
  return <><PageIntro eyebrow="Operational proof" title="Evidence centre" text="Illustrative browser-local records; sample images do not prove authenticity, GPS or verified timestamps."><Button onClick={()=>open("Upload evidence")}><Icon name="upload"/> Upload evidence</Button></PageIntro>
    <div className="filter-row">{["All","Weighing","Loading","Delivery","Maintenance"].map(x=><button className={filter===x?"active":""} onClick={()=>setFilter(x)} key={x}>{x}</button>)}</div>
    <div className="evidence-grid">{items.map((e,i)=><button key={e.id} className="evidence-card" onClick={()=>detail(e)}><div className={`evidence-art ${e.tone}`}><div className="fish-shape"><i/><span/></div><em>ILLUSTRATIVE {e.category.toUpperCase()}</em></div><div className="evidence-info"><span><small>{e.id} · {e.category}</small><Status>{e.status}</Status></span><b>{e.title}</b><p>{e.related} · Submitted by {e.by}</p></div></button>)}</div></>;
}

function Reports({currency,go}:any){
  const reports=[["Procurement performance","4,750 kg purchased","procurement"],["Stock movement","2,060 kg available","inventory"],["Sales & retailer balances",money(1974,currency),"sales"],["Cash reconciliation","−$12 combined variance","cashbook"],["Remittance register","1 awaiting review","cashbook"],["Fuel & maintenance",money(331,currency),"assets"],["Evidence exceptions","2 need review","evidence"],["Weekly target","79% of upper target","overview"]];
  return <><PageIntro eyebrow="Management reporting" title="Reports & Intelligence" text="Operational reporting and rule-based illustrative insights—no real AI service is connected."/>
    <div className="report-grid">{reports.map((r,i)=><button key={r[0]} onClick={()=>go(r[2])}><i><Icon name={nav[(i+1)%nav.length].icon}/></i><span><b>{r[0]}</b><small>{r[1]}</small></span><Icon name="arrow"/></button>)}</div>
    <Card className="intel"><div className="intel-head"><i>OH</i><div><span className="eyebrow">SIMULATED · RULE-BASED</span><div className="section-title">Ocean Harvest Intelligence</div><p>Illustrative observations generated from the shared demo records.</p></div><span className="demo-pill">No AI API</span></div><div className="insight-list">{[
      ["High","Buying advance ADV-220 requires reconciliation.","$352 remains after purchases and approved expenses.","procurement"],
      ["High","Dispatch and receipt weights differ.","TRF-109 arrived 12 kg below dispatch; human review is required.","transfers"],
      ["Medium","Daily remittance awaits review.","Kariba remittance RMT-082 for $700 is pending.","cashbook"],
      ["Medium","Purchase evidence is incomplete.","PUR-0841 does not have a weighing-scale photo.","evidence"],
      ["Low","Weekly purchases are above minimum target.","4,750 kg is within the configured 4–6 tonne band.","overview"],
      ["Medium","Boat maintenance is due soon.","MV Nyaminyami inspection MNT-024 is due 21 June.","assets"],
    ].map(x=><button key={x[1]} onClick={()=>go(x[3])}><span className={`severity ${x[0].toLowerCase()}`}>{x[0]}</span><span><b>{x[1]}</b><small>{x[2]}</small></span><Icon name="arrow"/></button>)}</div></Card></>;
}

function Settings({currency,setCurrency,toast}:any){
  const [species,setSpecies]=useState(["Bream","Kapenta","Tiger fish"]);
  const [forms,setForms]=useState(["Whole · iced","Form unconfirmed"]);
  const [locations,setLocations]=useState(["Kariba, Zimbabwe","Harare, Zimbabwe"]);
  const [methods,setMethods]=useState(["Cash","Bank transfer","Mobile money","Credit"]);
  const [categories,setCategories]=useState(["Fuel","Ice & coolers","Loading","Repairs","Crew meals"]);
  const tagEditor=(title:string,items:string[],set:any)=><Card><SectionTitle title={title} text="Click a tag to remove it"/><div className="tags">{items.map(x=><button key={x} onClick={()=>set(items.filter(i=>i!==x))}>{x}<Icon name="x" size={13}/></button>)}<button className="add-tag" onClick={()=>set([...items,`New ${title.toLowerCase().slice(0,-1)}`])}><Icon name="plus" size={14}/> Add</button></div></Card>;
  return <><PageIntro eyebrow="Workspace configuration" title="Business settings" text="Changes are a browser-local demonstration and do not alter real operational records."><Button onClick={()=>toast("Demo settings saved locally")}><Icon name="check"/> Save changes</Button></PageIntro>
    <div className="settings-grid"><Card><SectionTitle title="Procurement targets" text="Used by overview and reports"/><div className="settings-fields"><label><span>Weekly minimum (kg)</span><input defaultValue="4000"/></label><label><span>Weekly stretch (kg)</span><input defaultValue="6000"/></label><label><span>Operating days</span><select defaultValue="Mon–Sun"><option>Mon–Sun</option><option>Mon–Sat</option><option>Custom</option></select></label></div></Card><Card><SectionTitle title="Currency display" text="No exchange rates are assumed"/><div className="currency-switch">{["USD","ZAR","ZiG"].map(x=><button key={x} className={currency===x?"active":""} onClick={()=>setCurrency(x)}><b>{x}</b><span>{x==="USD"?"US Dollar":x==="ZAR"?"South African Rand":"Zimbabwe Gold"}</span></button>)}</div></Card></div>
    <div className="settings-grid">{tagEditor("Fish species",species,setSpecies)}{tagEditor("Product forms",forms,setForms)}{tagEditor("Locations",locations,setLocations)}{tagEditor("Payment methods",methods,setMethods)}{tagEditor("Expense categories",categories,setCategories)}</div></>;
}

export default function App() {
  const [page,setPage]=useState<PageKey>("overview");
  const [menu,setMenu]=useState(false);
  const [location,setLocation]=useState("All locations");
  const [role,setRole]=useState("Owner / Admin");
  const [currency,setCurrency]=useState("USD");
  const [modal,setModal]=useState<string|null>(null);
  const [selected,setSelected]=useState<Row|null>(null);
  const [toast,setToast]=useState("");
  const [search,setSearch]=useState("");
  const [activity,setActivity]=useState<Row[]>([]);
  const pageTitle=nav.find(x=>x.key===page)?.label;
  const notify=(msg:string)=>{setToast(msg);setModal(null);setTimeout(()=>setToast(""),2800)};
  const saveRecord=(type:string)=>{
    const reference = type==="Record purchase" ? "PUR-DEMO-001" : type==="Record dispatch" ? "TRF-DEMO-001" : type==="Add sale" ? "INV-DEMO-001" : type==="Record payment" ? "PAY-DEMO-001" : `DEMO-${String(activity.length+1).padStart(3,"0")}`;
    const record:Row = {
      reference,
      action:type,
      status:"Saved locally",
      recorded:"Just now",
      ...(type==="Record purchase" ? {batch:"BAT-DEMO-001",supplier:"Demo fisherman",species:"Bream",netWeight:"120 kg"} : {}),
      ...(type==="Record dispatch" ? {route:"Kariba → Harare",batch:"BAT-DEMO-001",dispatchedWeight:"120 kg"} : {}),
      ...(type==="Add sale" ? {retailer:"Demo retailer",saleWeight:"40 kg",paymentType:"Cash"} : {}),
      ...(type==="Record payment" ? {invoice:"INV-1458",amount:money(200,currency),newBalance:money(448,currency)} : {}),
    };
    setActivity(current=>[record,...current].slice(0,4));
    setModal(null);
    setSelected(record);
    setToast(`${type} saved to this demo session`);
    setTimeout(()=>setToast(""),2800);
  };
  const open=(x:string)=>setModal(x);
  const detail=(x:Row)=>setSelected(x);
  const content=useMemo(()=>{
    const props={currency,open,detail,go:(x:PageKey)=>setPage(x)};
    if(page==="overview") return <Overview {...props}/>;
    if(page==="procurement") return <Procurement {...props}/>;
    if(page==="inventory") return <Inventory {...props}/>;
    if(page==="transfers") return <Transfers {...props}/>;
    if(page==="sales") return <Sales {...props}/>;
    if(page==="cashbook") return <Cashbook {...props}/>;
    if(page==="assets") return <Assets {...props}/>;
    if(page==="team") return <Team role={role} setRole={setRole} detail={detail}/>;
    if(page==="evidence") return <Evidence open={open} detail={detail}/>;
    if(page==="reports") return <Reports currency={currency} go={(x:PageKey)=>setPage(x)}/>;
    return <Settings currency={currency} setCurrency={setCurrency} toast={notify}/>;
  },[page,currency,role]);
  return <div className="app">
    <aside className={menu?"open":""}>
      <div className="brand"><div className="logo"><svg viewBox="0 0 42 42"><path d="M7 21c7-9 17-10 27-2-8 0-12 4-15 8-4-2-8-3-12-2l4-4-4-4v4Z"/><path d="M8 30c8-3 16-1 24 4" /></svg></div><div><b>Ocean Harvest</b><span>OS · Powered by Tishande</span></div><button onClick={()=>setMenu(false)}><Icon name="x"/></button></div>
      <nav>{nav.map(n=><button key={n.key} className={page===n.key?"active":""} onClick={()=>{setPage(n.key);setMenu(false);window.scrollTo(0,0)}}><Icon name={n.icon}/><span>{n.label}</span>{n.key==="overview"&&<i className="nav-dot"/>}</button>)}</nav>
      <div className="sidebar-foot"><div className="role-user"><i>CM</i><span><b>Chipo Mhlanga</b><small>{role} · demo</small></span><Icon name="dots"/></div><p>Operational control<br/>from lake to retailer.</p><small>Powered by <b>Tishande</b></small></div>
    </aside>
    <div className="shell">
      <header><button className="mobile-menu" onClick={()=>setMenu(true)}><Icon name="menu"/></button><div className="mobile-title">{pageTitle}</div><div className="search"><Icon name="search"/><input value={search} onChange={e=>setSearch(e.target.value)} placeholder="Search records, people, references…"/><kbd>⌘ K</kbd></div><div className="top-actions"><span className="demo-pill">DEMO DATA</span><label className="location-select"><Icon name="grid"/><select value={location} onChange={e=>setLocation(e.target.value)}><option>All locations</option><option>Kariba</option><option>Harare</option></select></label><button className="notification" onClick={()=>notify("You have 6 operational alerts")}><Icon name="bell"/><i/></button><div className="online"><i/>Simulated online</div></div></header>
      <main>{search&&<div className="search-notice">Showing demo search context for “{search}”. Open a module to refine results.<button onClick={()=>setSearch("")}><Icon name="x" size={14}/></button></div>}{content}{activity.length>0&&<Card className="session-activity"><SectionTitle title="This demo session" text="Browser-local actions recorded during this visit"/><div>{activity.map((item,i)=><button key={i} onClick={()=>setSelected(item)}><i><Icon name="check"/></i><span><b>{String(item.action)}</b><small>{String(item.reference)} · {String(item.recorded)}</small></span><Icon name="arrow"/></button>)}</div></Card>}<footer><span>Ocean Harvest OS · Demonstration workspace</span><b>Powered by Tishande</b><span>All names and records are fictional</span></footer></main>
    </div>
    {menu&&<div className="menu-scrim" onClick={()=>setMenu(false)}/>}
    {modal&&<SimpleForm type={modal} onClose={()=>setModal(null)} onSave={saveRecord}/>}
    {selected&&<Modal title={String(selected.ref||selected.invoice||selected.batch||selected.id||selected.name||selected.reference||"Record details")} onClose={()=>setSelected(null)}><div className="detail-grid">{Object.entries(selected).filter(([k,v])=>!React.isValidElement(v)&&k!=="tone").map(([k,v])=><div key={k}><span>{k.replace(/([A-Z])/g," $1")}</span><b>{String(v)}</b></div>)}</div><div className="detail-note"><Icon name="image"/><span><b>Illustrative record</b>This is fictional demo data and not verified operational evidence.</span></div><div className="form-actions"><Button variant="secondary" onClick={()=>setSelected(null)}>Close</Button><Button onClick={()=>notify("Record marked for demo review")}>Mark reviewed</Button></div></Modal>}
    {toast&&<div className="toast"><i><Icon name="check"/></i><span><b>Action complete</b>{toast}</span></div>}
  </div>;
}
