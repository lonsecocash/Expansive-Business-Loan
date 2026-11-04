const $ = (id) => document.getElementById(id);

const amount = $("amount");
const rate = $("rate");
const months = $("months");

const money = (n) =>
  n.toLocaleString("en-US", { style: "currency", currency: "USD", maximumFractionDigits: 0 });

function calculate() {
  const P = Number(amount.value);
  const annual = Number(rate.value);
  const n = Number(months.value);
  const r = annual / 12 / 100;

  // Standard EMI formula: P * r * (1+r)^n / ((1+r)^n - 1)
  const emi = r === 0 ? P / n : (P * r * Math.pow(1 + r, n)) / (Math.pow(1 + r, n) - 1);
  const total = emi * n;

  $("amountOut").textContent = money(P);
  $("rateOut").textContent = annual.toFixed(1) + "%";
  const y = Math.floor(n / 12), m = n % 12;
  $("monthsOut").textContent = [y ? y + (y === 1 ? " year" : " years") : "", m ? m + (m === 1 ? " month" : " months") : ""].filter(Boolean).join(" ");
  $("emi").textContent = money(emi);
  $("interest").textContent = money(total - P);
  $("total").textContent = money(total);
}

[amount, rate, months].forEach((el) => el.addEventListener("input", calculate));

// "Try these numbers" buttons fill the calculator and the form
document.querySelectorAll(".pick").forEach((btn) => {
  btn.addEventListener("click", () => {
    amount.value = btn.dataset.amount;
    rate.value = btn.dataset.rate;
    months.value = btn.dataset.months;
    $("loanType").value = btn.dataset.type;
    calculate();
    window.scrollTo({ top: 0, behavior: "smooth" });
  });
});

// Application form (front-end only: connect to a backend or form service to receive data)
$("applyForm").addEventListener("submit", (e) => {
  e.preventDefault();
  const msg = $("formMsg");
  const fields = ["name", "email", "phone"].map($);
  let valid = true;

  fields.forEach((f) => {
    const ok = f.value.trim() !== "" && (f.type !== "email" || /^\S+@\S+\.\S+$/.test(f.value));
    f.classList.toggle("invalid", !ok);
    if (!ok) valid = false;
  });

  if (!valid) {
    msg.className = "err";
    msg.textContent = "Fill in your name, a valid email and your phone number.";
    return;
  }

  msg.className = "ok";
  msg.textContent = "Thanks, " + $("name").value.trim() + ". We'll contact you within one business day.";
  e.target.reset();
});

$("year").textContent = new Date().getFullYear();
calculate();
