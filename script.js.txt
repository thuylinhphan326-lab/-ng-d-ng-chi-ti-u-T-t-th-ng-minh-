function formatMoney(num) {
    return num.toLocaleString("vi-VN") + " VNĐ";
}

function createPlan() {
    let money = Number(document.getElementById("moneyInput").value);
    if (!money || money <= 0) {
        alert("Vui lòng nhập số tiền hợp lệ!");
        return;
    }

    // PHÂN CHIA % HỢP LÝ – KHÔNG BAO GIỜ BỊ LẺ
    let categories = {
        "Chi tiêu gia đình & chuẩn bị Tết": 0.22,
        "Quà biếu & thăm hỏi": 0.18,
        "Lì xì Tết": 0.20,
        "Trang phục & cá nhân": 0.15,
        "Ăn uống – đi chơi": 0.10,
        "Học tập đầu năm": 0.08,
        "Quỹ dự phòng": 0.07
    };

    let planHTML = "";

    for (let [name, percent] of Object.entries(categories)) {
        let amount = Math.floor(money * percent / 1000) * 1000; // làm tròn ngàn
        let items = getItems(name, amount);

        planHTML += `
            <div class="card">
                <h2>${name} — ${formatMoney(amount)}</h2>
                <ul>${items.join("")}</ul>
            </div>
        `;
    }

    document.getElementById("plan").innerHTML = planHTML;
}

function getItems(category, total) {
    // chia nhỏ theo mức độ ưu tiên
    let ratios;
    if (category === "Quà biếu & thăm hỏi") {
        ratios = {
            "Biếu bố mẹ": 0.32,
            "Biếu ông bà": 0.28,
            "Biếu họ hàng": 0.20,
            "Biếu thầy cô": 0.12,
            "Tiền thăm hỏi": 0.08
        };
    } else if (category === "Lì xì Tết") {
        ratios = {
            "Trẻ em": 0.30,
            "Anh chị em": 0.20,
            "Bố mẹ": 0.30,
            "Bạn bè": 0.15,
            "Phát sinh": 0.05
        };
    } else {
        ratios = {
            "Khoản chính 1": 0.35,
            "Khoản chính 2": 0.30,
            "Khoản phụ 1": 0.20,
            "Khoản phụ 2": 0.15
        };
    }

    let list = [];
    for (let [name, r] of Object.entries(ratios)) {
        let money = Math.floor(total * r / 1000) * 1000;
        list.push(`<li>${name} — ${formatMoney(money)}</li>`);
    }
    return list;
}
