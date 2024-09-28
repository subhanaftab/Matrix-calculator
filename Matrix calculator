const checkbox = document.getElementById("checkbox");
const hamburger = document.querySelector(".menuicon");
const input = document.getElementsByClassName("Userinput")[0];
const sendBtn = document.querySelector(".sendIcon");
const chatList = document.querySelector(".chatList");
const userName = document.querySelector(".userName");
const promptcards = document.querySelector(".promptcards");
const newChat = document.querySelectorAll("#new-Chat");

try {
  newChat.forEach((chatBtn) => {
    chatBtn.addEventListener("click", () => {
      alert("Your old chat will be deleted.");
      localStorage.clear();
      chatList.innerHTML = "";
      chatList.style.display = "none";
      userName.style.display = "flex";
      promptcards.style.display = "flex";
    });
  });

  function scrollToBottom() {
    chatList.scrollTop = chatList.scrollHeight; // Scroll to the bottom of the chat
  }

  let responseApi;
  let userMessage;

  // Store user message and API response in localStorage
  function storeChatInLocalStorage(userMsg, apiMsg) {
    let chats = JSON.parse(localStorage.getItem("chats")) || [];
    chats.push({ userMessage: userMsg, apiMessage: apiMsg });
    localStorage.setItem("chats", JSON.stringify(chats));
  }

  // Retrieve chat history from localStorage
  function loadChatFromLocalStorage() {
    let chats = JSON.parse(localStorage.getItem("chats")) || [];
    chats.forEach((chat) => {
      if (chat) {
        outGoingMsgHtml(chat.userMessage);
        responseApi = chat.apiMessage;
        incomingMsgHtml();
        chatList.style.display = "flex";
        userName.style.display = "none";
        promptcards.style.display = "none";
        document.querySelectorAll("#loades").forEach((span) => {
          span.innerHTML = "";
        });
      } else {
        chatList.style.display = "none";
        userName.style.display = "flex";
        promptcards.style.display = "flex";
      }
    });
  }

  // Scroll to bottom after loading previous chats
  window.onload = function () {
    loadChatFromLocalStorage();
    scrollToBottom();
  };

  checkbox.addEventListener("click", () => {
    document.body.classList.toggle("darkmode");
  });

  hamburger.addEventListener("click", () => {
    document.querySelector("ul").classList.toggle("active");
  });

  input.addEventListener("input", () => {
    if (input.value.trim() !== "") {
      sendBtn.style.display = "block";
    } else {
      sendBtn.style.display = "none";
    }
  });

  sendBtn.addEventListener("click", addOutGoingMsg);

  function addOutGoingMsg(e) {
    e.preventDefault();
    userMessage = input.value.trim(); // Trim to avoid extra white spaces
    if (userMessage.length === 0) return; // Check empty message
    userName.style.display = "none";
    promptcards.style.display = "none";
    chatList.style.display = "flex";
    outGoingMsgHtml(userMessage);
    sendBtn.style.display = "none";
    generateApiResponse(userMessage);
    input.value = "";
    scrollToBottom();
  }

  input.addEventListener("keydown", (e) => {
    if (e.key === "Enter" && input.value.trim() !== "") {
      addOutGoingMsg(e);
    }
  });

  const apiKey = "AIzaSyCog2hjME1crLrlXVB3WedPc9puolTJGs0";
  const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash:generateContent?key=${apiKey}`;

  const generateApiResponse = async (userMsg) => {
    try {
      const response = await fetch(apiUrl, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
          contents: [
            {
              role: "user",
              parts: [{ text: userMsg }],
            },
          ],
        }),
      });

      document.querySelectorAll("#loades").forEach((span) => {
        span.innerHTML = "";
      });

      const data = await response.json();
      const ApiResponse =
        data?.candidates?.[0]?.content?.parts?.[0]?.text ||
        "No response from API.";
      responseApi = ApiResponse;

      // Save chat to localStorage
      storeChatInLocalStorage(userMsg, responseApi);
      incomingMsgHtml();
    } catch (e) {
      alert(e.message);
    }
  };

  const outGoingMsgHtml = (userMessage) => {
    const div = document.createElement("div");
    div.className = "message outgoing";
    const img = document.createElement("img");
    img.alt = "User img";
    img.className = "images";
    img.src = "/Images/user.jpg";
    const p = document.createElement("p");
    p.className = "outgoingText";
    p.innerText = userMessage;
    div.appendChild(img);
    div.appendChild(p);

    chatList.appendChild(div);

    // Loading spinner
    chatList.insertAdjacentHTML(
      "beforeend",
      ` <span id="loades"> <div class="loading">
      <img src="/Images/gemini.svg" alt="gemini" class="Loaderimg" />
      <div class="loaders">
        <div class="loader"></div>
        <div class="loader"></div>
        <div class="loader"></div>
      </div>
    </div>
    </span>`
    );
  };

  const incomingMsgHtml = () => {
    const div = document.createElement("div");
    div.className = "message incoming";
    const span = document.createElement("div");
    span.className = "textSpan";
    const img = document.createElement("img");
    img.src = "/Images/gemini.svg";
    img.alt = "gemini";
    const p = document.createElement("p");
    p.className = "incomingText";
    p.innerText = responseApi;
    span.appendChild(img);
    span.appendChild(p);
    div.appendChild(span);
    div.insertAdjacentHTML(
      "beforeend",
      ` <div class="icons">
      <span class="icon material-symbols-rounded"> thumb_up </span>
      <span class="icon material-symbols-rounded"> thumb_down </span>
      <span class="icon material-symbols-rounded"> refresh </span>
      <span class="icon material-symbols-rounded"> more_vert </span>
    </div>`
    );
    chatList.appendChild(div);
    scrollToBottom();
  };
} catch (error) {
  alert(error.message);
}
