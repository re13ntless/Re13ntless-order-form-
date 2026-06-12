import { useState } from "react";

const FORMSPREE_URL = "https://formspree.io/f/mvznwroo";

// ── Constants ────────────────────────────────────────────────────────────────

const SERVICES = [
  { id: "screen", icon: "🎨", title: "Screen Printing", desc: "Bold, vibrant prints. Best for 12+ pieces with 1–6 colors." },
  { id: "embroidery", icon: "🧵", title: "Embroidery", desc: "Stitched into the fabric. Professional look for hats, polos & jackets." },
  { id: "dtf", icon: "✨", title: "DTF Transfer", desc: "Full-color, photo-quality prints. No minimums. Works on any fabric." },
  { id: "promo", icon: "🎁", title: "Promotional Products", desc: "Branded pens, bags, drinkware, and more for events or giveaways." },
];

const GARMENTS = {
  screen: ["T-Shirt (Short Sleeve)","T-Shirt (Long Sleeve)","Hoodie","Crewneck Sweatshirt","Tank Top","Youth T-Shirt","Long Sleeve Youth","Pocket Tee","Performance / Dri-Fit Shirt","Other"],
  embroidery: ["Structured Hat / Cap","Dad Hat (Unstructured)","Beanie","Polo Shirt","Button-Down / Work Shirt","Jacket / Zip-Up","Hoodie","Bag / Tote","Towel / Blanket","Other"],
  dtf: ["T-Shirt (Short Sleeve)","T-Shirt (Long Sleeve)","Hoodie","Crewneck Sweatshirt","Performance / Polyester Shirt","Youth T-Shirt","Onesie / Infant","Tote Bag","Jacket","Other"],
  promo: ["Tote Bag","Drinkware / Tumbler","Hat / Cap","Pen / Stylus","Notebook / Journal","Lanyard","Umbrella","Koozi / Can Cooler","Drawstring Bag","Other / Not Sure"],
};

const PRINT_LOCATIONS = {
  screen: ["Full Front (up to 12in wide)","Full Back (up to 12in wide)","Left Chest (3-4in logo)","Right Chest","Front & Back","Left Chest + Back","Sleeve (left or right)","Hood","Custom / Multiple Locations"],
  embroidery: ["Left Chest (most common)","Right Chest","Front Center of Hat","Left Side of Hat","Back of Hat","Back of Shirt / Jacket","Sleeve","Back Collar / Nape","Both Sides"],
  dtf: ["Full Front","Full Back","Left Chest","Right Chest","Front & Back","Sleeve","Custom / Multiple"],
  promo: ["One side","Both sides","Wraparound","Not sure"],
};

const SIZES_ADULT = ["XS","S","M","L","XL","2XL","3XL","4XL"];
const SIZES_YOUTH = ["YXS","YS","YM","YL","YXL"];
const HAT_SIZES = ["One Size Fits Most","S/M","L/XL","Custom Fitted"];
const FABRIC_TYPES = ["100% Cotton","Cotton/Poly Blend (50/50)","Cotton/Poly Blend (60/40)","100% Polyester / Performance","Tri-Blend","Not sure — we're buying blanks from you"];
const GARMENT_COLORS = ["White","Black","Navy","Royal Blue","Red","Heather Gray","Sport Gray","Dark Heather","Forest Green","Maroon","Purple","Orange","Yellow","Light Blue","Pink","Other (describe below)"];
const SCREEN_COLORS = ["1 color","2 colors","3 colors","4 colors","5 colors","6 colors","Full color (DTF recommended instead)","Not sure — help me decide"];
const FINISH_OPTIONS = {
  screen: ["Standard (default)","Puff / 3D Ink","Metallic / Foil","Glow in the Dark","Discharge (vintage/soft feel)","Water-based (eco-friendly)"],
  embroidery: ["Standard flat stitch","3D Puff embroidery","Chenille","Not sure"],
  dtf: ["Matte finish","Glossy finish","No preference"],
  promo: ["Standard logo imprint","Full color print","Laser engraved","Embroidered","Not sure"],
};
const ARTWORK_STATUS = [
  { val: "ready_vector", label: "Yes — I have a vector file (.ai, .eps, .svg, .pdf)" },
  { val: "ready_highres", label: "Yes — I have a high-res image (.png, .jpg, 300dpi+)" },
  { val: "ready_low", label: "I have a logo but it's low quality / screenshot" },
  { val: "partial", label: "I have something rough — needs cleanup" },
  { val: "none", label: "No artwork — I need design help from scratch" },
];
const HOW_HEARD = ["Google Search","Facebook / Instagram","Word of mouth / referral","Returning customer","Saw a shirt someone was wearing","Local event / trade show","314shirts.com website","Other"];

const emptyAdultQty = () => SIZES_ADULT.reduce((a, s) => ({ ...a, [s]: "" }), {});
const emptyYouthQty = () => SIZES_YOUTH.reduce((a, s) => ({ ...a, [s]: "" }), {});

const defaultForm = {
  service: "",
  firstName: "", lastName: "", email: "", phone: "", org: "", role: "", howHeard: "",
  garment: "", garmentBrand: "", garmentColor: "", garmentColor2: "", fabricType: "", providingOwn: "",
  printLocation: "", printLocation2: "", printColors: "", finishType: "", designColors: "",
  adultQty: emptyAdultQty(), youthQty: emptyYouthQty(), hatQty: "", singleQty: "",
  artworkStatus: "", artworkDesc: "", designText: "", hasLogo: "", preferenceStyle: "",
  threadColors: "", stitchWidth: "", stitchHeight: "", backingType: "",
  dtfFinish: "", transferSize: "", gangSheet: "",
  needByDate: "", isRush: "", deliveryMethod: "", shippingAddress: "", budget: "", additionalNotes: "",
};

// ── Main App ─────────────────────────────────────────────────────────────────
export default function App() {
  const [form, setForm] = useState(defaultForm);
  const [step, setStep] = useState(0);
  const [errors, setErrors] = useState({});
  const [submitted, setSubmitted] = useState(false);
  const [submitting, setSubmitting] = useState(false);
  const [submitError, setSubmitError] = useState("");

  const set = (k, v) => setForm(f => ({ ...f, [k]: v }));
  const setAQty = (s, v) => setForm(f => ({ ...f, adultQty: { ...f.adultQty, [s]: v } }));
  const setYQty = (s, v) => setForm(f => ({ ...f, youthQty: { ...f.youthQty, [s]: v } }));

  const service = SERVICES.find(s => s.id === form.service);
  const isHat = form.garment && (form.garment.toLowerCase().includes("hat") || form.garment.toLowerCase().includes("cap") || form.garment.toLowerCase().includes("beanie"));
  const isPromo = form.service === "promo";

  const totalAdult = Object.values(form.adultQty).reduce((s, v) => s + (parseInt(v) || 0), 0);
  const totalYouth = Object.values(form.youthQty).reduce((s, v) => s + (parseInt(v) || 0), 0);
  const totalHat = parseInt(form.hatQty) || 0;
  const totalSingle = parseInt(form.singleQty) || 0;
  const totalPieces = isHat || isPromo ? (totalHat || totalSingle) : (totalAdult + totalYouth);

  const steps = [
    { id: "service", title: "Choose Your Service", progress: 0 },
    { id: "contact", title: "Your Information", progress: 13 },
    { id: "garment", title: "Garment Details", progress: 26 },
    { id: "print", title: "Decoration Details", progress: 39 },
    { id: "quantities", title: "Sizes & Quantities", progress: 52 },
    { id: "artwork", title: "Artwork & Design", progress: 65 },
    ...(form.service === "embroidery" ? [{ id: "emb_detail", title: "Embroidery Specifics", progress: 75 }] : []),
    ...(form.service === "dtf" ? [{ id: "dtf_detail", title: "Transfer Specifics", progress: 75 }] : []),
    { id: "logistics", title: "Timeline & Delivery", progress: 87 },
    { id: "review", title: "Review & Submit", progress: 100 },
  ];

  const currentStep = steps[step];
  const isLast = step === steps.length - 1;

  const validate = () => {
    const e = {};
    const s = currentStep.id;
    if (s === "service" && !form.service) e.service = "Please choose a service.";
    if (s === "contact") {
      if (!form.firstName.trim()) e.firstName = "Required";
      if (!form.lastName.trim()) e.lastName = "Required";
      if (!form.email.trim() || !/\S+@\S+\.\S+/.test(form.email)) e.email = "Valid email required";
      if (!form.phone.trim()) e.phone = "Required";
    }
    if (s === "garment") {
      if (!form.garment) e.garment = "Select a garment";
      if (!form.garmentColor) e.garmentColor = "Select a color";
      if (!form.providingOwn) e.providingOwn = "Please answer";
    }
    if (s === "print") {
      if (!form.printLocation) e.printLocation = "Select a location";
      if (form.service === "screen" && !form.printColors) e.printColors = "How many colors?";
      if (!form.finishType) e.finishType = "Select a finish";
    }
    if (s === "quantities" && totalPieces < 1 && !form.hatQty && !form.singleQty) e.quantities = "Enter at least one quantity";
    if (s === "artwork" && !form.artworkStatus) e.artworkStatus = "Please answer";
    if (s === "logistics" && !form.deliveryMethod) e.deliveryMethod = "Select an option";
    setErrors(e);
    return Object.keys(e).length === 0;
  };

  const handleSubmit = async () => {
    setSubmitting(true);
    setSubmitError("");
    try {
      const adultSizes = SIZES_ADULT.filter(s => parseInt(form.adultQty[s]) > 0)
        .map(s => `${s}: ${form.adultQty[s]}`).join(", ");
      const youthSizes = SIZES_YOUTH.filter(s => parseInt(form.youthQty[s]) > 0)
        .map(s => `${s}: ${form.youthQty[s]}`).join(", ");

      const payload = {
        _subject: `New Order Request — ${form.firstName} ${form.lastName} (${service?.title})`,
        "Service Type": service?.title || "",
        "Customer Name": `${form.firstName} ${form.lastName}`,
        "Email": form.email,
        "Phone": form.phone,
        "Organization": form.org || "N/A",
        "Role": form.role || "N/A",
        "How They Found Us": form.howHeard || "N/A",
        "Garment": form.garment,
        "Brand / Style": form.garmentBrand || "N/A",
        "Garment Color": form.garmentColor + (form.garmentColor2 ? ` + ${form.garmentColor2}` : ""),
        "Fabric Type": form.fabricType || "N/A",
        "Sourcing Garments": form.providingOwn === "we_source" ? "Re13ntless sources" : form.providingOwn === "providing" ? "Customer providing" : "TBD",
        "Decoration Location": form.printLocation + (form.printLocation2 ? ` + ${form.printLocation2}` : ""),
        "Print Colors": form.printColors || "N/A",
        "Finish / Style": form.finishType || "N/A",
        "Design Colors": form.designColors || "N/A",
        "Adult Sizes": adultSizes || (form.hatQty ? `Total hats: ${form.hatQty}` : form.singleQty ? `Total qty: ${form.singleQty}` : "N/A"),
        "Youth Sizes": youthSizes || "N/A",
        "Total Pieces": totalPieces || "N/A",
        "Artwork Status": ARTWORK_STATUS.find(a => a.val === form.artworkStatus)?.label || "N/A",
        "Has Existing Logo": form.hasLogo || "N/A",
        "Design Description": form.artworkDesc || "N/A",
        "Text in Design": form.designText || "N/A",
        "Style Preference": form.preferenceStyle || "N/A",
        "Thread Colors": form.threadColors || "N/A",
        "Embroidery Size": form.stitchWidth ? `${form.stitchWidth} x ${form.stitchHeight}` : "N/A",
        "Transfer Size": form.transferSize || "N/A",
        "Gang Sheet": form.gangSheet || "N/A",
        "Need By Date": form.needByDate || "Flexible",
        "Rush Order": form.isRush === "yes" ? "YES - Rush" : form.isRush === "no" ? "No" : "TBD",
        "Delivery Method": form.deliveryMethod === "pickup" ? "Pickup at shop" : form.deliveryMethod === "ship" ? "Ship to customer" : "TBD",
        "Shipping Address": form.shippingAddress || "N/A",
        "Budget Range": form.budget || "Not provided",
        "Additional Notes": form.additionalNotes || "None",
      };

      const res = await fetch(FORMSPREE_URL, {
        method: "POST",
        headers: { "Content-Type": "application/json", "Accept": "application/json" },
        body: JSON.stringify(payload),
      });

      if (res.ok) {
        setSubmitted(true);
      } else {
        setSubmitError("Something went wrong. Please call us at 314-270-8558 or email Info@re13ntless.com");
      }
    } catch {
      setSubmitError("Could not submit. Please call us at 314-270-8558 or email Info@re13ntless.com");
    } finally {
      setSubmitting(false);
    }
  };

  const next = () => { if (!validate()) return; setStep(s => Math.min(s + 1, steps.length - 1)); window.scrollTo(0, 0); };
  const back = () => { setErrors({}); setStep(s => Math.max(s - 1, 0)); window.scrollTo(0, 0); };

  return (
    <div style={S.page}>
      <Header />
      {step > 0 && (
        <>
          <div style={S.progressWrap}><div style={{ ...S.progressBar, width: `${currentStep.progress}%` }} /></div>
          <div style={S.stepLabel}>Step {step} of {steps.length - 1} — <strong>{currentStep.title}</strong></div>
        </>
      )}
      <div style={S.card}>
        {currentStep.id === "service" && <StepService form={form} set={set} errors={errors} />}
        {currentStep.id === "contact" && <StepContact form={form} set={set} errors={errors} />}
        {currentStep.id === "garment" && <StepGarment form={form} set={set} errors={errors} />}
        {currentStep.id === "print" && <StepPrint form={form} set={set} errors={errors} />}
        {currentStep.id === "quantities" && <StepQuantities form={form} set={set} setAQty={setAQty} setYQty={setYQty} errors={errors} isHat={isHat} isPromo={isPromo} totalAdult={totalAdult} totalYouth={totalYouth} totalPieces={totalPieces} />}
        {currentStep.id === "artwork" && <StepArtwork form={form} set={set} errors={errors} />}
        {currentStep.id === "emb_detail" && <StepEmbroidery form={form} set={set} errors={errors} />}
        {currentStep.id === "dtf_detail" && <StepDTF form={form} set={set} errors={errors} />}
        {currentStep.id === "logistics" && <StepLogistics form={form} set={set} errors={errors} />}
        {currentStep.id === "review" && <StepReview form={form} service={service} totalPieces={totalPieces} />}

        <div style={S.navRow}>
          {step > 0 && <button style={S.btnBack} onClick={back}>← Back</button>}
          <div style={{ flex: 1 }} />
          {isLast
            ? <button style={{ ...S.btnPrimary, opacity: submitting ? 0.7 : 1 }} onClick={handleSubmit} disabled={submitting}>
                {submitting ? "Sending..." : "Submit Order Request ✓"}
              </button>
            : <button style={S.btnPrimary} onClick={next}>{step === 0 ? "Start My Order →" : "Continue →"}</button>
          }
        </div>
        {submitError && (
          <p style={{ color: "#c0392b", fontSize: 13, marginTop: 12, textAlign: "center" }}>{submitError}</p>
        )}
      </div>
      <div style={S.footerBar}>
        <div style={S.footerInner}>
          <span>📍 9931 Lin Ferry Dr, St. Louis, MO</span>
          <span>📞 314-270-8558</span>
          <span>✉️ Info@re13ntless.com</span>
          <span>🕐 Mon–Thu 9am–5pm</span>
        </div>
      </div>
    </div>
  );
}

// ── Step Components ───────────────────────────────────────────────────────────

function StepService({ form, set, errors }) {
  return (
    <div>
      <h2 style={S.cardTitle}>What can we create for you?</h2>
      <p style={S.cardSub}>Re13ntless Customs · St. Louis screen printing, embroidery, DTF & promo products. Choose your service below.</p>
      <div style={S.serviceGrid}>
        {SERVICES.map(svc => (
          <button key={svc.id} style={{ ...S.serviceCard, ...(form.service === svc.id ? S.serviceCardActive : {}) }} onClick={() => set("service", svc.id)}>
            <div style={S.serviceIcon}>{svc.icon}</div>
            <div style={S.serviceTitle}>{svc.title}</div>
            <div style={S.serviceDesc}>{svc.desc}</div>
          </button>
        ))}
      </div>
      {errors.service && <p style={S.err}>{errors.service}</p>}
      <div style={S.tipBox}>
        💡 <strong>Not sure which to pick?</strong> Screen printing is best for bulk orders. Embroidery is ideal for hats and polos. DTF handles full-color photos with no minimums. Call us at <strong>314-270-8558</strong> and we'll help you decide!
      </div>
    </div>
  );
}

function StepContact({ form, set, errors }) {
  return (
    <div>
      <h2 style={S.cardTitle}>Tell us about yourself</h2>
      <p style={S.cardSub}>We'll use this to send your quote and keep you updated every step of the way.</p>
      <Row2>
        <Field label="First Name *" error={errors.firstName}><input style={inp(errors.firstName)} value={form.firstName} placeholder="Jane" onChange={e => set("firstName", e.target.value)} /></Field>
        <Field label="Last Name *" error={errors.lastName}><input style={inp(errors.lastName)} value={form.lastName} placeholder="Smith" onChange={e => set("lastName", e.target.value)} /></Field>
      </Row2>
      <Row2>
        <Field label="Email Address *" error={errors.email}><input style={inp(errors.email)} value={form.email} placeholder="jane@email.com" onChange={e => set("email", e.target.value)} /></Field>
        <Field label="Phone Number *" error={errors.phone}><input style={inp(errors.phone)} value={form.phone} placeholder="(314) 555-0100" onChange={e => set("phone", e.target.value)} /></Field>
      </Row2>
      <Row2>
        <Field label="Organization / Team / Business" hint="School, company, sports team, etc."><input style={inp()} value={form.org} placeholder="Optional" onChange={e => set("org", e.target.value)} /></Field>
        <Field label="Your Role" hint="Helps us talk to the right person">
          <select style={inp()} value={form.role} onChange={e => set("role", e.target.value)}>
            <option value="">Select...</option>
            {["Owner / Decision Maker","Coach / Athletic Director","Event Organizer","Marketing / Brand Manager","Individual / Personal Order","Fundraiser Organizer","Other"].map(r => <option key={r}>{r}</option>)}
          </select>
        </Field>
      </Row2>
      <Field label="How did you hear about Re13ntless Customs?">
        <select style={inp()} value={form.howHeard} onChange={e => set("howHeard", e.target.value)}>
          <option value="">Select...</option>
          {HOW_HEARD.map(h => <option key={h}>{h}</option>)}
        </select>
      </Field>
      <div style={S.tipBox}>🔒 Your info is only used to fulfill your order. We never sell or share your data.</div>
    </div>
  );
}

function StepGarment({ form, set, errors }) {
  const garments = GARMENTS[form.service] || [];
  return (
    <div>
      <h2 style={S.cardTitle}>What are we decorating?</h2>
      <p style={S.cardSub}>Tell us about the garment or item. Not sure on brand? We source quality blanks from S&S Activewear and SanMar.</p>
      <Row2>
        <Field label="Item Type *" error={errors.garment}>
          <select style={inp(errors.garment)} value={form.garment} onChange={e => set("garment", e.target.value)}>
            <option value="">Select...</option>
            {garments.map(g => <option key={g}>{g}</option>)}
          </select>
        </Field>
        <Field label="Preferred Brand / Style" hint="e.g. Gildan 5000, Bella+Canvas 3001 — or leave blank">
          <input style={inp()} value={form.garmentBrand} placeholder="Leave blank if unsure" onChange={e => set("garmentBrand", e.target.value)} />
        </Field>
      </Row2>
      <Row2>
        <Field label="Garment Color *" error={errors.garmentColor} hint="Primary color">
          <select style={inp(errors.garmentColor)} value={form.garmentColor} onChange={e => set("garmentColor", e.target.value)}>
            <option value="">Select color...</option>
            {GARMENT_COLORS.map(c => <option key={c}>{c}</option>)}
          </select>
        </Field>
        <Field label="Second Garment Color?" hint="If you need two colors in the same order">
          <select style={inp()} value={form.garmentColor2} onChange={e => set("garmentColor2", e.target.value)}>
            <option value="">None</option>
            {GARMENT_COLORS.map(c => <option key={c}>{c}</option>)}
          </select>
        </Field>
      </Row2>
      {form.service !== "embroidery" && form.service !== "promo" && (
        <Field label="Fabric Type" hint="Affects which ink or transfer works best">
          <select style={inp()} value={form.fabricType} onChange={e => set("fabricType", e.target.value)}>
            <option value="">Select fabric...</option>
            {FABRIC_TYPES.map(f => <option key={f}>{f}</option>)}
          </select>
        </Field>
      )}
      <Field label="Are you providing your own garments, or should we source them? *" error={errors.providingOwn}>
        <div style={S.radioGroup}>
          {[
            { v: "we_source", l: "Re13ntless Customs will source the blanks for me" },
            { v: "providing", l: "I'm bringing / shipping my own garments" },
            { v: "unsure", l: "Not sure yet — let's talk options" },
          ].map(({ v, l }) => (
            <label key={v} style={S.radioLabel}><input type="radio" name="providingOwn" value={v} checked={form.providingOwn === v} onChange={() => set("providingOwn", v)} style={{ marginRight: 8 }} />{l}</label>
          ))}
        </div>
      </Field>
      <div style={S.tipBox}>💡 We source from <strong>S&S Activewear</strong> (Next Level, Bella+Canvas, Comfort Colors, Adidas, Columbia) and <strong>SanMar</strong> (Nike, District, The North Face, OGIO). Just tell us what you like!</div>
    </div>
  );
}

function StepPrint({ form, set, errors }) {
  const locations = PRINT_LOCATIONS[form.service] || [];
  const finishes = FINISH_OPTIONS[form.service] || [];
  return (
    <div>
      <h2 style={S.cardTitle}>Decoration details</h2>
      <p style={S.cardSub}>This is where we nail down the technical details so your order comes out exactly right.</p>
      <Row2>
        <Field label="Primary Location *" error={errors.printLocation}>
          <select style={inp(errors.printLocation)} value={form.printLocation} onChange={e => set("printLocation", e.target.value)}>
            <option value="">Select location...</option>
            {locations.map(l => <option key={l}>{l}</option>)}
          </select>
        </Field>
        <Field label="Second Location?" hint="Only if you need decoration in 2+ spots">
          <select style={inp()} value={form.printLocation2} onChange={e => set("printLocation2", e.target.value)}>
            <option value="">None</option>
            {locations.map(l => <option key={l}>{l}</option>)}
          </select>
        </Field>
      </Row2>
      {form.service === "screen" && (
        <Field label="How many colors in your design? *" error={errors.printColors} hint="Each color = one screen. White counts as a color on dark shirts.">
          <select style={inp(errors.printColors)} value={form.printColors} onChange={e => set("printColors", e.target.value)}>
            <option value="">Select...</option>
            {SCREEN_COLORS.map(c => <option key={c}>{c}</option>)}
          </select>
        </Field>
      )}
      {(form.service === "screen" || form.service === "embroidery") && (
        <Field label="Design colors" hint="Use names, hex codes, or Pantone numbers if you know them">
          <input style={inp()} value={form.designColors} placeholder="e.g. Purple #6B2D8B, black, white" onChange={e => set("designColors", e.target.value)} />
        </Field>
      )}
      <Field label={form.service === "embroidery" ? "Stitch Style *" : "Finish / Special Effect *"} error={errors.finishType}>
        <select style={inp(errors.finishType)} value={form.finishType} onChange={e => set("finishType", e.target.value)}>
          <option value="">Select...</option>
          {finishes.map(f => <option key={f}>{f}</option>)}
        </select>
      </Field>
      {form.service === "screen" && <div style={S.tipBox}>💡 <strong>Dark shirt tip:</strong> Printing on black or dark garments requires a white underbase layer — this counts as one of your ink colors. We'll always confirm before printing.</div>}
      {form.service === "embroidery" && <div style={S.tipBox}>🧵 <strong>Embroidery tip:</strong> Very thin lines and gradients don't embroider well. Our team will review your artwork and suggest fixes before we start — no extra charge.</div>}
      {form.service === "dtf" && <div style={S.tipBox}>✨ <strong>DTF tip:</strong> DTF handles unlimited colors, photos, and gradients. Works on cotton, poly, and blends. No minimums required!</div>}
    </div>
  );
}

function StepQuantities({ form, set, setAQty, setYQty, errors, isHat, isPromo, totalAdult, totalYouth, totalPieces }) {
  return (
    <div>
      <h2 style={S.cardTitle}>Sizes & quantities</h2>
      <p style={S.cardSub}>Enter how many of each size you need. You can adjust before we finalize.</p>
      {errors.quantities && <p style={S.err}>{errors.quantities}</p>}
      {isHat || isPromo ? (
        <Field label={isPromo ? "Total quantity needed" : "Total hats needed"}>
          <input style={{ ...inp(), maxWidth: 160 }} type="number" min="0" value={form.hatQty || form.singleQty} placeholder="e.g. 24" onChange={e => isPromo ? set("singleQty", e.target.value) : set("hatQty", e.target.value)} />
        </Field>
      ) : (
        <>
          <SectionHead>Adult Sizes</SectionHead>
          <div style={S.sizeGrid}>
            {SIZES_ADULT.map(s => <SizeBox key={s} label={s} value={form.adultQty[s]} onChange={v => setAQty(s, v)} />)}
          </div>
          {totalAdult > 0 && <p style={S.totalLine}>Adult subtotal: <strong>{totalAdult}</strong></p>}
          <SectionHead>Youth Sizes (optional)</SectionHead>
          <div style={{ ...S.sizeGrid, gridTemplateColumns: "repeat(5, 1fr)" }}>
            {SIZES_YOUTH.map(s => <SizeBox key={s} label={s} value={form.youthQty[s]} onChange={v => setYQty(s, v)} />)}
          </div>
          {totalYouth > 0 && <p style={S.totalLine}>Youth subtotal: <strong>{totalYouth}</strong></p>}
        </>
      )}
      {totalPieces > 0 && (
        <div style={S.totalBox}>
          <span>Total pieces: </span>
          <strong style={{ fontSize: 20, color: PLUM }}>{totalPieces}</strong>
          {form.service === "screen" && totalPieces < 12 && <span style={S.warnText}> · Screen printing minimum is 12 pieces — we'll confirm options with you.</span>}
          {form.service === "screen" && totalPieces >= 12 && totalPieces < 24 && <span style={{ color: "#555", fontSize: 13 }}> · Pricing gets better at 24, 48, and 72 pieces!</span>}
          {form.service === "dtf" && <span style={{ color: "#555", fontSize: 13 }}> · DTF has no minimum — any quantity works!</span>}
        </div>
      )}
      <div style={{ marginTop: 16 }}>
        <Field label="Size notes?" hint="e.g. 'Need 2XL in black only' or 'youth sizes for kids ages 8-12'">
          <textarea style={{ ...inp(), height: 60, resize: "vertical" }} value={form.additionalNotes} placeholder="Optional..." onChange={e => set("additionalNotes", e.target.value)} />
        </Field>
      </div>
    </div>
  );
}

function StepArtwork({ form, set, errors }) {
  return (
    <div>
      <h2 style={S.cardTitle}>Artwork & design</h2>
      <p style={S.cardSub}>Don't worry if your artwork isn't perfect — we work with everything from polished vector files to napkin sketches.</p>
      <Field label="What's the status of your artwork? *" error={errors.artworkStatus}>
        <div style={S.radioGroup}>
          {ARTWORK_STATUS.map(({ val, label }) => (
            <label key={val} style={S.radioLabel}><input type="radio" name="artworkStatus" value={val} checked={form.artworkStatus === val} onChange={() => set("artworkStatus", val)} style={{ marginRight: 8 }} />{label}</label>
          ))}
        </div>
      </Field>
      <Field label="Do you have an existing logo / brand?">
        <div style={S.radioGroup}>
          {[{ v: "yes", l: "Yes — I have a brand logo" }, { v: "no", l: "No — this is a new design" }, { v: "sorta", l: "Sort of — needs work" }].map(({ v, l }) => (
            <label key={v} style={S.radioLabel}><input type="radio" name="hasLogo" value={v} checked={form.hasLogo === v} onChange={() => set("hasLogo", v)} style={{ marginRight: 8 }} />{l}</label>
          ))}
        </div>
      </Field>
      <Field label="Describe your design" hint="Be as detailed as possible — text, colors, style, images, placement notes">
        <textarea style={{ ...inp(), height: 96, resize: "vertical" }} value={form.artworkDesc} placeholder="e.g. 'Team name RE13NTLESS in bold white on front, number on back in purple and black, distressed athletic style. Tough and bold.'" onChange={e => set("artworkDesc", e.target.value)} />
      </Field>
      <Row2>
        <Field label="Colors in your design" hint="Names, hex codes, or Pantone numbers">
          <input style={inp()} value={form.designColors} placeholder="e.g. Plum purple, black, white" onChange={e => set("designColors", e.target.value)} />
        </Field>
        <Field label="Any text to include?" hint="Exact spelling matters — double-check names & numbers">
          <input style={inp()} value={form.designText} placeholder="e.g. Re13ntless Customs · St. Louis" onChange={e => set("designText", e.target.value)} />
        </Field>
      </Row2>
      <Field label="Style preference">
        <select style={inp()} value={form.preferenceStyle} onChange={e => set("preferenceStyle", e.target.value)}>
          <option value="">No preference / surprise me</option>
          {["Bold & aggressive","Clean & minimal","Vintage / retro","Athletic / sporty","Classic / professional","Fun & playful","Match existing brand exactly"].map(s => <option key={s}>{s}</option>)}
        </select>
      </Field>
      {(form.artworkStatus === "ready_low" || form.artworkStatus === "partial" || form.artworkStatus === "none") && (
        <div style={S.tipBox}>🎨 <strong>No worries!</strong> Our in-house design team can build your artwork from scratch or clean up what you have. We'll send a mockup for approval before anything gets printed. Design fees may apply depending on complexity.</div>
      )}
      {form.artworkStatus === "ready_vector" && (
        <div style={{ ...S.tipBox, background: "#f3eef8", borderColor: PLUM }}>✅ <strong>Vector file is perfect!</strong> This gives us the cleanest output. We'll confirm colors and sizing when we review your file.</div>
      )}
    </div>
  );
}

function StepEmbroidery({ form, set }) {
  return (
    <div>
      <h2 style={S.cardTitle}>Embroidery specifics</h2>
      <p style={S.cardSub}>A few extra details help us digitize your design correctly the first time.</p>
      <Row2>
        <Field label="Approximate design width" hint={'In inches — left chest logos are typically 3-4"'}>
          <input style={inp()} value={form.stitchWidth} placeholder='e.g. 3.5"' onChange={e => set("stitchWidth", e.target.value)} />
        </Field>
        <Field label="Approximate design height" hint="In inches">
          <input style={inp()} value={form.stitchHeight} placeholder='e.g. 2"' onChange={e => set("stitchHeight", e.target.value)} />
        </Field>
      </Row2>
      <Field label="Thread colors" hint="List colors in your design. We use Madeira and Isacord thread. Max 6 colors recommended.">
        <textarea style={{ ...inp(), height: 72, resize: "vertical" }} value={form.threadColors} placeholder="e.g. White text, plum purple background, black outline" onChange={e => set("threadColors", e.target.value)} />
      </Field>
      <Field label="Backing preference" hint="Most customers can leave this on standard — we pick what's right for the fabric">
        <select style={inp()} value={form.backingType} onChange={e => set("backingType", e.target.value)}>
          <option value="">Standard (we choose what's best)</option>
          <option>Cutaway (most durable, for structured items)</option>
          <option>Tearaway (softer feel, for stable fabrics)</option>
          <option>Water-soluble (for stretchy or delicate fabrics)</option>
          <option>Not sure — please advise</option>
        </select>
      </Field>
      <div style={S.tipBox}>🧵 <strong>Digitizing note:</strong> Every new embroidery design gets digitized before production. We'll send a digital proof for your approval. Thin lines under 1.5mm and gradients can't be embroidered — we'll flag any issues before we start.</div>
    </div>
  );
}

function StepDTF({ form, set }) {
  return (
    <div>
      <h2 style={S.cardTitle}>DTF transfer details</h2>
      <p style={S.cardSub}>Just a couple extra questions to make sure your transfers press perfectly.</p>
      <Row2>
        <Field label="Approximate transfer size" hint={'Width x height in inches. Full front is typically 10-12" wide.'}>
          <input style={inp()} value={form.transferSize} placeholder='e.g. 10" x 10"' onChange={e => set("transferSize", e.target.value)} />
        </Field>
        <Field label="Finish preference">
          <select style={inp()} value={form.dtfFinish} onChange={e => set("dtfFinish", e.target.value)}>
            <option value="">Standard / Matte (default)</option>
            <option>Matte finish</option>
            <option>Glossy finish</option>
            <option>No preference</option>
          </select>
        </Field>
      </Row2>
      <Field label="Do you need gang sheets?" hint="Gang sheets fit multiple designs on one sheet — saves cost if you have several small designs">
        <div style={S.radioGroup}>
          {[{ v: "no", l: "No — single design, standard sizing" }, { v: "yes", l: "Yes — I have multiple designs or small logos" }, { v: "unsure", l: "Not sure — please advise" }].map(({ v, l }) => (
            <label key={v} style={S.radioLabel}><input type="radio" name="gangSheet" value={v} checked={form.gangSheet === v} onChange={() => set("gangSheet", v)} style={{ marginRight: 8 }} />{l}</label>
          ))}
        </div>
      </Field>
      <div style={S.tipBox}>✨ <strong>DTF pro tip:</strong> DTF transfers work on cotton, polyester, blends, leather, and more. Pressed at 300-325°F for 12-15 seconds. If you're pressing yourself, we'll include application instructions with your order.</div>
    </div>
  );
}

function StepLogistics({ form, set, errors }) {
  return (
    <div>
      <h2 style={S.cardTitle}>Timeline & delivery</h2>
      <p style={S.cardSub}>Let us know your deadline and how you'd like to receive your order.</p>
      <Row2>
        <Field label="Need-by date" hint="Leave blank if flexible — standard turnaround is 10-14 business days">
          <input style={inp()} type="date" value={form.needByDate} onChange={e => set("needByDate", e.target.value)} />
        </Field>
        <Field label="Is this a rush order?">
          <div style={S.radioGroup}>
            {[{ v: "no", l: "No — standard turnaround is fine" }, { v: "yes", l: "Yes — I need this faster (rush fees may apply)" }, { v: "unsure", l: "Not sure — let's talk" }].map(({ v, l }) => (
              <label key={v} style={S.radioLabel}><input type="radio" name="isRush" value={v} checked={form.isRush === v} onChange={() => set("isRush", v)} style={{ marginRight: 8 }} />{l}</label>
            ))}
          </div>
        </Field>
      </Row2>
      <Field label="How would you like to receive your order? *" error={errors.deliveryMethod}>
        <div style={S.radioGroup}>
          {[{ v: "pickup", l: "Pickup at 9931 Lin Ferry Dr, St. Louis, MO (Mon-Thu 9am-5pm)" }, { v: "ship", l: "Ship to me (shipping cost added to invoice)" }, { v: "unsure", l: "Not sure yet" }].map(({ v, l }) => (
            <label key={v} style={S.radioLabel}><input type="radio" name="deliveryMethod" value={v} checked={form.deliveryMethod === v} onChange={() => set("deliveryMethod", v)} style={{ marginRight: 8 }} />{l}</label>
          ))}
        </div>
      </Field>
      {form.deliveryMethod === "ship" && (
        <Field label="Shipping address">
          <textarea style={{ ...inp(), height: 72, resize: "vertical" }} value={form.shippingAddress} placeholder="Street, City, State, ZIP" onChange={e => set("shippingAddress", e.target.value)} />
        </Field>
      )}
      <Field label="Budget range" hint="Totally optional — helps us recommend the best options">
        <select style={inp()} value={form.budget} onChange={e => set("budget", e.target.value)}>
          <option value="">Prefer not to say</option>
          {["Under $100","$100 – $250","$250 – $500","$500 – $1,000","$1,000 – $2,500","$2,500+"].map(b => <option key={b}>{b}</option>)}
        </select>
      </Field>
      <Field label="Anything else we should know?">
        <textarea style={{ ...inp(), height: 80, resize: "vertical" }} value={form.additionalNotes} placeholder="Special requests, event details, packaging needs, reorder info, etc." onChange={e => set("additionalNotes", e.target.value)} />
      </Field>
      <div style={S.tipBox}>📦 <strong>Standard turnaround:</strong> 10-14 business days from artwork approval. Rush options available. We'll confirm your exact in-hands date when we send your quote.</div>
    </div>
  );
}

function StepReview({ form, service, totalPieces }) {
  const rows = [
    ["Service", service?.title],
    ["Name", `${form.firstName} ${form.lastName}`],
    ["Email", form.email],
    ["Phone", form.phone],
    form.org && ["Organization", form.org],
    form.role && ["Role", form.role],
    form.howHeard && ["How They Found Us", form.howHeard],
    ["Garment", form.garment],
    form.garmentBrand && ["Brand/Style", form.garmentBrand],
    ["Garment Color", form.garmentColor + (form.garmentColor2 ? ` + ${form.garmentColor2}` : "")],
    form.fabricType && ["Fabric", form.fabricType],
    ["Sourcing Garments", form.providingOwn === "we_source" ? "Re13ntless sources" : form.providingOwn === "providing" ? "Customer providing" : "TBD"],
    ["Decoration Location", form.printLocation + (form.printLocation2 ? ` + ${form.printLocation2}` : "")],
    form.printColors && ["Print Colors", form.printColors],
    form.finishType && ["Finish", form.finishType],
    form.designColors && ["Design Colors", form.designColors],
    ["Total Pieces", totalPieces || "—"],
    ["Artwork Status", ARTWORK_STATUS.find(a => a.val === form.artworkStatus)?.label || "—"],
    form.artworkDesc && ["Design Description", form.artworkDesc],
    form.designText && ["Text in Design", form.designText],
    form.preferenceStyle && ["Style Preference", form.preferenceStyle],
    form.threadColors && ["Thread Colors", form.threadColors],
    form.stitchWidth && ["Embroidery Size", `${form.stitchWidth} x ${form.stitchHeight}`],
    form.transferSize && ["Transfer Size", form.transferSize],
    form.needByDate && ["Need By", form.needByDate],
    ["Rush Order", form.isRush === "yes" ? "Yes" : form.isRush === "no" ? "No" : "TBD"],
    ["Delivery", form.deliveryMethod === "pickup" ? "Pickup — 9931 Lin Ferry Dr" : form.deliveryMethod === "ship" ? "Ship to customer" : "TBD"],
    form.shippingAddress && ["Ship To", form.shippingAddress],
    form.budget && ["Budget Range", form.budget],
    form.additionalNotes && ["Notes", form.additionalNotes],
  ].filter(Boolean).filter(r => r[1]);

  return (
    <div>
      <h2 style={S.cardTitle}>Review your order</h2>
      <p style={S.cardSub}>Everything look right? Hit Submit and we'll send your custom quote within 1 business day.</p>
      <div style={S.reviewBox}>
        {rows.map(([label, value]) => (
          <div key={label} style={S.reviewRow}>
            <span style={S.reviewLabel}>{label}</span>
            <span style={S.reviewValue}>{value}</span>
          </div>
        ))}
      </div>
      <div style={S.tipBox}>✅ <strong>What happens next:</strong> We'll review your request and reach out within 1 business day with a detailed quote. Once approved, we collect a deposit and kick off production. Questions? Call <strong>314-270-8558</strong>.</div>
    </div>
  );
}

function SuccessScreen({ form, service, totalPieces, onReset }) {
  return (
    <div style={S.page}>
      <Header />
      <div style={S.card}>
        <div style={S.successBadge}>✓</div>
        <h2 style={{ ...S.cardTitle, textAlign: "center" }}>Order Request Submitted!</h2>
        <p style={{ ...S.cardSub, textAlign: "center" }}>
          Thanks, <strong>{form.firstName}</strong>! We'll review your <strong>{service?.title}</strong> request and reach out to <strong>{form.email}</strong> within 1 business day.
        </p>
        <div style={S.reviewBox}>
          <div style={S.reviewRow}><span style={S.reviewLabel}>Service</span><span style={S.reviewValue}>{service?.title}</span></div>
          <div style={S.reviewRow}><span style={S.reviewLabel}>Garment</span><span style={S.reviewValue}>{form.garment}</span></div>
          {totalPieces > 0 && <div style={S.reviewRow}><span style={S.reviewLabel}>Total Pieces</span><span style={{ ...S.reviewValue, color: PLUM, fontWeight: 700 }}>{totalPieces}</span></div>}
          {form.needByDate && <div style={S.reviewRow}><span style={S.reviewLabel}>Need By</span><span style={S.reviewValue}>{form.needByDate}</span></div>}
        </div>
        <div style={{ ...S.tipBox, textAlign: "center", marginBottom: 20 }}>
          📞 Questions? Call or text us at <strong>314-270-8558</strong> or email <strong>Info@re13ntless.com</strong>
        </div>
        <button style={S.btnPrimary} onClick={onReset}>Place Another Order</button>
      </div>
      <div style={S.footerBar}>
        <div style={S.footerInner}>
          <span>📍 9931 Lin Ferry Dr, St. Louis, MO</span>
          <span>📞 314-270-8558</span>
          <span>✉️ Info@re13ntless.com</span>
          <span>🕐 Mon–Thu 9am–5pm</span>
        </div>
      </div>
    </div>
  );
}

// ── Shared helpers ────────────────────────────────────────────────────────────
function Header() {
  return (
    <div style={S.header}>
      <img
        src="https://static.wixstatic.com/media/31b708_cc88305002e64348b5de63de7dad8bb7~mv2.png/v1/crop/x_5,y_1,w_6615,h_1025/fill/w_660,h_102,al_c,q_85,usm_0.66_1.00_0.01,enc_avif,quality_auto/314%20shirts%20re13ntless%20customs%20together%20promo%20.png"
        alt="Re13ntless Customs 314Shirts"
        style={S.logoImg}
        onError={e => { e.target.style.display = "none"; }}
      />
      <p style={S.tagline}>St. Louis Screen Printing · Embroidery · DTF · Promo Products</p>
    </div>
  );
}
function Row2({ children }) { return <div style={S.row2}>{children}</div>; }
function SectionHead({ children }) { return <h4 style={S.sectionHead}>{children}</h4>; }
function SizeBox({ label, value, onChange }) {
  return (
    <div style={S.sizeCell}>
      <label style={S.sizeLabel}>{label}</label>
      <input style={S.sizeInput} type="number" min="0" placeholder="0" value={value} onChange={e => onChange(e.target.value)} />
    </div>
  );
}
function Field({ label, hint, error, children }) {
  return (
    <div style={S.fieldWrap}>
      <label style={S.label}>{label}</label>
      {hint && <span style={S.hint}>{hint}</span>}
      {children}
      {error && <span style={S.err}>{error}</span>}
    </div>
  );
}

// ── Design tokens ─────────────────────────────────────────────────────────────
const PLUM = "#6B2D8B";
const PLUM_DARK = "#4e1f66";
const PLUM_LIGHT = "#f3eef8";
const BLACK = "#111";
const GRAY = "#555";
const BORDER = "#d1d5db";
const ERR = "#c0392b";

const inp = (error) => ({
  width: "100%", padding: "10px 12px", border: `1.5px solid ${error ? ERR : BORDER}`,
  borderRadius: 6, fontSize: 15, color: BLACK, background: "#fff",
  boxSizing: "border-box", outline: "none", fontFamily: "inherit",
});

const S = {
  page: { minHeight: "100vh", background: "#f3f4f6", paddingBottom: 80, fontFamily: "'Segoe UI', system-ui, sans-serif", color: BLACK },
  header: { maxWidth: 720, margin: "0 auto", padding: "20px 16px 8px", textAlign: "center" },
  logoImg: { maxWidth: 320, height: "auto", marginBottom: 6 },
  tagline: { margin: 0, fontSize: 12, color: GRAY, letterSpacing: "0.3px" },
  progressWrap: { maxWidth: 720, margin: "12px auto 4px", height: 5, background: "#e5e7eb", borderRadius: 99, overflow: "hidden", padding: "0 16px", boxSizing: "border-box" },
  progressBar: { height: "100%", background: PLUM, borderRadius: 99, transition: "width 0.4s ease" },
  stepLabel: { maxWidth: 720, margin: "0 auto 12px", fontSize: 13, color: GRAY, padding: "0 16px" },
  card: { background: "#fff", borderRadius: 12, padding: "28px 24px", maxWidth: 720, margin: "0 auto 16px", boxShadow: "0 2px 12px rgba(0,0,0,0.07)", marginLeft: 16, marginRight: 16 },
  cardTitle: { margin: "0 0 6px", fontSize: 22, fontWeight: 800, color: BLACK },
  cardSub: { margin: "0 0 22px", color: GRAY, fontSize: 14, lineHeight: 1.5 },
  serviceGrid: { display: "grid", gridTemplateColumns: "repeat(2, 1fr)", gap: 12, marginBottom: 16 },
  serviceCard: { background: "#f9fafb", border: `2px solid ${BORDER}`, borderRadius: 10, padding: "16px 12px", cursor: "pointer", textAlign: "center", transition: "all 0.15s" },
  serviceCardActive: { border: `2px solid ${PLUM}`, background: PLUM_LIGHT },
  serviceIcon: { fontSize: 26, marginBottom: 6 },
  serviceTitle: { fontWeight: 700, fontSize: 14, marginBottom: 4, color: BLACK },
  serviceDesc: { fontSize: 12, color: GRAY, lineHeight: 1.4 },
  tipBox: { background: "#fffbeb", border: "1px solid #fde68a", borderRadius: 8, padding: "12px 14px", fontSize: 13, color: "#78350f", marginTop: 14, lineHeight: 1.5 },
  row2: { display: "grid", gridTemplateColumns: "1fr 1fr", gap: 14, marginBottom: 12 },
  fieldWrap: { display: "flex", flexDirection: "column", gap: 4, marginBottom: 14 },
  label: { fontSize: 13, fontWeight: 600, color: "#333" },
  hint: { fontSize: 12, color: "#888", marginTop: -2 },
  err: { fontSize: 12, color: ERR },
  radioGroup: { display: "flex", flexDirection: "column", gap: 8, marginTop: 4 },
  radioLabel: { fontSize: 14, color: "#333", cursor: "pointer", display: "flex", alignItems: "flex-start", lineHeight: 1.4 },
  sizeGrid: { display: "grid", gridTemplateColumns: "repeat(4, 1fr)", gap: 10, marginBottom: 8 },
  sizeCell: { display: "flex", flexDirection: "column", alignItems: "center", gap: 4 },
  sizeLabel: { fontSize: 11, fontWeight: 700, color: GRAY, textTransform: "uppercase" },
  sizeInput: { width: "100%", padding: "8px 4px", textAlign: "center", border: `1.5px solid ${BORDER}`, borderRadius: 6, fontSize: 15, color: BLACK, boxSizing: "border-box", fontFamily: "inherit" },
  totalLine: { fontSize: 13, color: GRAY, margin: "4px 0 12px" },
  totalBox: { background: PLUM_LIGHT, border: `1px solid ${PLUM}`, borderRadius: 8, padding: "12px 16px", fontSize: 15, marginTop: 8 },
  warnText: { color: "#92400e", fontSize: 13 },
  sectionHead: { margin: "16px 0 10px", fontSize: 12, fontWeight: 700, textTransform: "uppercase", letterSpacing: "0.8px", color: PLUM },
  reviewBox: { background: "#f9fafb", border: `1px solid ${BORDER}`, borderRadius: 8, padding: "14px", marginBottom: 16 },
  reviewRow: { display: "flex", justifyContent: "space-between", fontSize: 13, padding: "6px 0", borderBottom: "1px solid #f0f0f0", gap: 16 },
  reviewLabel: { fontWeight: 600, color: "#444", flexShrink: 0 },
  reviewValue: { color: "#333", textAlign: "right" },
  navRow: { display: "flex", alignItems: "center", marginTop: 24, gap: 12 },
  btnPrimary: { padding: "13px 24px", background: PLUM, color: "#fff", border: "none", borderRadius: 8, fontSize: 15, fontWeight: 700, cursor: "pointer" },
  btnBack: { padding: "13px 18px", background: "transparent", color: GRAY, border: `1.5px solid ${BORDER}`, borderRadius: 8, fontSize: 15, cursor: "pointer" },
  successBadge: { width: 56, height: 56, borderRadius: "50%", background: PLUM_LIGHT, color: PLUM, fontSize: 26, fontWeight: 900, display: "flex", alignItems: "center", justifyContent: "center", margin: "0 auto 16px" },
  footerBar: { background: PLUM, padding: "14px 16px", marginTop: 24 },
  footerInner: { maxWidth: 720, margin: "0 auto", display: "flex", flexWrap: "wrap", gap: "8px 20px", justifyContent: "center", color: "#fff", fontSize: 12 },
};
