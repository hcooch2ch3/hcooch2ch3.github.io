---
layout: post
title: Content Hugging VS Compression Resistance
tags: 
- content hugging priority
- compression resistance priority
- autolayout
---

Autolayout 스터디를 참여하면서 공부한 Content Hugging과 Compression Resistance에 대해서 정리해보고자 한다. (좋은 과제를 내준 Jake에게 감사합니다.)

Content Hugging과 Compression Resistance은 상황의존적이다. 뷰가 늘어나거나 줄어들어야하는 상황에서만 작용하기 때문이다. Content Hugging은 뷰의 크기를 늘려야 할때 작용한다. Compression Resistance은 뷰의 크기를 줄여야 할때 작용한다.

Content Hugging은 View가 Content를 감싸려고 하는 성질이다. 그래서 View가 Cotent를 감싸려고 하기 때문에 View가 커져야 하는 상황에서 커지지 않게 한다. 반면에 Compression Resistance는 View가 압축에 저항하는 성질이다. 그래서 View가 작아져야 하는 상황에서 작아지지 않게 한다.

사실 Content Hugging과 Compression Resistance 그 자체는 레이아웃을 구현하는데 직접 사용되는 것은 아니고 일종의 관념(?)일 뿐이다. 실제론 Content Hugging Priority와 Compression Resistance Priority가 레이아웃을 구현하는데 사용된다. 뷰 크기가 확장되어야 할때 어떤 뷰를 확장시킬건지를 결정하는 값이 Content Hugging Priority이고, 뷰 크기가 축소되어야 할때 어떤 뷰를 축소시킬건지를 결정하는 값이 Compression Resistance Priority 이다. Priority는 1~1000 범위의 값으로 설정 가능하다. Priority가 1000일때 가장 크기가 변환되기 어렵고, 1일때 가장 크기가 변환되기 쉽다.

다음은 Compression Resistance Priority에 따라 어떻게 뷰 크기가 바뀌는지에 대한 예제이다.

![CR0](/assets/images/CR0.png){: width="40%"}

오른쪽의 파란색 버튼을 누르면 아래의 그림처럼 애플 스토어의 주소 Label 이 나타난다. 그리고 주소 Label을 완전히 나타내기 위해 왼쪽 Label의 크기가 축소된다. 

![CR1](/assets/images/CR1.png){: width="40%"}

위 예제에 대한 View Controller 코드는 다음과 같다.
```
class ViewController: UITableViewController {

    @IBOutlet var nameLabels: [UILabel]!
    @IBOutlet var addressLabels: [UILabel]!
    
    override func viewDidLoad() {
        super.viewDidLoad()
    }
    
    override func tableView(_ tableView: UITableView, accessoryButtonTappedForRowWith indexPath: IndexPath) {
        let row = indexPath.row
        let addressLabel = addressLabels[row]
        let nameLabel = nameLabels[row]
        addressLabel.isHidden.toggle()
        if addressLabel.isHidden {
            nameLabel.setContentCompressionResistancePriority(UILayoutPriority(751), for: NSLayoutConstraint.Axis.horizontal)
        } else {
            nameLabel.setContentCompressionResistancePriority(UILayoutPriority(749), for: NSLayoutConstraint.Axis.horizontal)
        }
    }
    
}
```
tableView(_:accessoryButtonTappedForRowWith:)에 addressLabel을 나타나거나 사라지게 하고, nameLabel의 Compression Resistance Priority가 바뀌도록 구현하였다. 그래서 파란색 버튼을 누르면 주소 Label이 나오고 지점명 Label이 축소된다.

addressLabel의 기본 Compression Resistance Priority는 750이며, 변하지 않는다. 하지만 nameLabel(지점명 Label)의 Compression Resistance Priority는 addressLabel이 사라지면 751, 나타나면 749로 설정된다. 따라서 addressLabel이 사라지면 nameLabel의 Compression Resistance Priority가 addressLabel보다 높으므로 nameLabel 크기가 원래대로 돌아오고, addressLabel이 나타나면 nameLabel의 Compression Resistance Priority가 addressLabel보다 낮으므로 nameLabel 크기가 축소된다.

이처럼 Compression Resistance Priority와 Content Hugging Priority을 잘 설정하면, 뷰의 크기가 변화해야 할때 어떤 뷰 크기를 바꿀건지 우선순위를 결정할 수 있다.
