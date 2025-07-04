# Homework.ai
function payNow() {
  const options = {
    key: "rzp_test_YourKeyID", // Replace with your Razorpay Key ID
    amount: 4900, // ₹49 in paisa
    currency: "INR",
    name: "Homework AI",
    description: "AI Assignment Generator",
    handler: function (response) {
      alert("✅ Payment successful!");
      generateHomework();
    },
    prefill: {
      name: "Student",
      email: "student@example.com"
    },
    theme: {
      color: "#3399cc"
    }
  };
  const rzp = new Razorpay(options);
  rzp.open();
}

async function generateHomework() {
  const studentClass = document.getElementById('class').value;
  const subject = document.getElementById('subject').value;
  const topic = document.getElementById('topic').value;
  const output = document.getElementById('output');

  output.innerHTML = "Generating your assignment... 🧠✍️";

  const prompt = `Write a school assignment on the topic "${topic}" for class ${studentClass} ${subject}. Make it simple and well formatted with heading, introduction, 2-3 points, and conclusion.`;

  const response = await fetch("https://api.openai.com/v1/completions", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      "Authorization": "Bearer YOUR_OPENAI_API_KEY"
    },
    body: JSON.stringify({
      model: "text-davinci-003",
      prompt: prompt,
      max_tokens: 500,
      temperature: 0.7
    })
  });

  const data = await response.json();
  output.innerHTML = `<h3>Your Assignment:</h3><p>${data.choices[0].text}</p>`;
}
